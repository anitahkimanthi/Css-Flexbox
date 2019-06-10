<template>
  <div class="commentsCount">
    <div class="row">
      <div class="col-12 col-md-3 offset-md-9">
        <b>{{ comments.length }} comments</b>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'commentsCount',
  data() {
    return {
      comments: [],
    }
  },

  mounted() {
    if (localStorage.getItem('comments')) {
      try {
        this.comments = JSON.parse(localStorage.getItem('comments'))
      } catch (e) {
        localStorage.AddItem('comments')
      }
    }
    if (localStorage.getItem('replies')) {
      try {
        this.replies = JSON.parse(localStorage.getItem('replies'))
      } catch (e) {
        localStorage.AddItem('replies')
      }
    }
  },

  methods: {
    // commentbox methods and daata storage
    postComment() {
      if (!this.newComment) {
        return
      }

      this.comments.push(this.newComment)
      this.newComment = ''
      this.saveComments()
    },

    deleteComment(index) {
      this.comments.splice(index, 1)
      this.saveComments()
    },

    saveComments() {
      let parsed = JSON.stringify(this.comments)
      localStorage.setItem('comments', parsed)
    },
  }
}
</script>

<style>
</style>
