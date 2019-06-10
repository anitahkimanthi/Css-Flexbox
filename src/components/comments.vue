<template>
  <!-- reply and posts -->
  <div class="comments">
    <div class="card">
      <div
        class="card-body"
        v-for="(comment, index) in comments"
        v-bind:key="index"
      >
        <p class="card-text comment">{{ comment }}</p>
        <button
          type="button"
          class="btn btn-sm red"
          @click="deleteComment(index)"
        >
          <font-awesome-icon class="icon" icon="trash-alt"/>Delete
        </button>

        <button type="button" class="btn btn-sm" @click="toogleReply">
          <font-awesome-icon class="icon" icon="reply"/>
          Reply {{ replies.length }}
        </button>
      </div>
    </div>
    <div class="repliespost">
      <replies />
    </div>
  </div>
</template>
<script>
import replies from '@/components/replies.vue'
export default {
  name: 'home',
  components: {
    replies
  },

  data() {
    return {
      comments: []
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
    }
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
