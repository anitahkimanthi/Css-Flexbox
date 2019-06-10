<template>
  <div class="commentForm">
    <form>
      <div class="form-group row no-gutters">
        <label>
          <h3>Leave a comment</h3>
        </label>
        <textarea
          class="form-control"
          placeholder="Enter your comment here ..."
          v-model="newComment"
          @keyup.enter="postComment"
        ></textarea>
        <button class="btn replybtn" type="submit" @click="postComment">
          Send
        </button>
      </div>
    </form>
  </div>
</template>
<script>
export default {
  name: 'commentForm',

  data() {
    return {
      comments: []
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

    saveComments() {
      let parsed = JSON.stringify(this.comments)
      localStorage.setItem('comments', parsed)
    }
  }
}
</script>
<style>
textarea {
  width: 50%;
}
.replybtn {
  padding: 10px;
  width: 100%;
  background: #314753 !important;
  color: #fff !important;
  float: right;
  margin-top: 10px;
}
</style>