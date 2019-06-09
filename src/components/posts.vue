<template>
  <div class="messages">
    <div class="card">
      <div v-bind:key="comment" class="card-body" v-for="(comment,n) in comments">
        <p class="card-text">{{comment}}</p>
        <button type="button" class="btn btn-sm red" @click="deleteComment(n)">
          <font-awesome-icon class="icon" icon="trash-alt"/>Delete
        </button>
        <button type="button" class="btn btn-sm">
          <font-awesome-icon class="icon" icon="reply"/>Reply
        </button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      comments: [],
      newComment: null
    }
  },

  mounted() {
    if (localStorage.getItem('comments')) {
      try {
        this.comments = JSON.parse(localStorage.getItem('comments'))
      } catch (e) {
        localStorage.removeItem('comments')
      }
    }
  },

  methods: {
    postComment() {
      if (!this.newComment) return
      this.comments.push(this.newComment)
      this.newComment = ''
      this.saveComments()
    }
  },

  deleteComment(x) {
    this.comments.splice(x, 1)
    this.saveComments()
  },

  saveComments() {
    let parsed = JSON.stringify(this.comments)
    localStorage.setItem('comments', parsed)
  }
}
</script>
<style>
.card {
  border: none !important;
  background: none;
}
.icon {
  margin-right: 10px !important;
}
</style>
