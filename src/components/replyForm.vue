<template>
  <div class="replybox">
    <!-- message in put for reply -->
    <form class="col-6 offset-md-1" v-show="OpenReply">
      <div class="form-group row no-gutters">
        <textarea
          class="form-control"
          placeholder="Reply here ..."
          @keyup.enter="postReply"
          v-show="OpenReply"
          v-model="newReply"
        ></textarea>
        <button class="btn postreply" type="submit" @click="postReply">
          Post
        </button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: 'replyForm',

  data() {
    return {
      comments: [],
      replies: [],
      OpenReply: false,
      newReply: null
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
    postReply() {
      if (!this.newReply) {
        return
      }
      this.replies.push(this.newReply)
      this.newReply = ''
      this.saveReply()
    },

    deleteReply(index) {
      this.replies.splice(index, 1)
      this.saveReply()
    },

    saveReply() {
      let parsed = JSON.stringify(this.replies)
      localStorage.setItem('replies', parsed)
    },

    toogleReply() {
      this.OpenReply = !this.OpenReply
    }
  }
}
</script>

<style>
.postreply {
  background: #314753 !important;
  width: 100%;
  color: #fff !important;
  margin-top: 20px;
}
</style>
