<template>
  <div class="reply" v-show="OpenReply">
    <div class="col-11 offset-md-1">
      <div class="card" v-for="(reply, index) in replies" v-bind:key="reply">
        <div class="card-body">
          <p class="card-text">{{ reply }}</p>
          <button type="button" class="btn btn-sm" @click="deleteReply(index)">
            <font-awesome-icon class="icon" icon="trash-alt"/>Delete
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'replies',

  data() {
    return {
      replies: [],
      newReply: null
    }
  },

  mounted() {
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
<style></style>
