<template>
  <div class="reply col-11 offset-md-1" v-show="showReplyBox">
      <form >
      <div class="form-group row no-gutters">
        <label>Leave a comment</label>
        <textarea
          class="form-control"
          placeholder="Enter your comment here ..."
          v-model="newReply"
          @keyup.enter="postReply"
        ></textarea>
      </div>
    </form>
    <div class="card">
      <div class="card-body">
        <p class="card-text" v-for="(reply, n) in replies" v-bind:key="reply">{{replies}}</p>
        <a href="#" class="btn" @click="deleteReply(index)">
          <font-awesome-icon class="icon" icon="trash-alt"/>Delete
        </a>
      </div>
    </div>
  </div>
</template>
<script>
export default {
    name:"commentForm",
    
      data() {
    return {
      replies: [],
      newReply: null,
    }
  },

  mounted() {
    if (localStorage.getItem('replies')) {
      try {
        this.reply = JSON.parse(localStorage.getItem('replies'))
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
    },

    saveReply() {
      let parsed = JSON.stringify(this.replies)
      localStorage.setItem('replies', parsed)
    }
  }
}
</script>
<style></style>
