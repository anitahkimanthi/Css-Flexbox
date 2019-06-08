<template>
  <div class="content">
    <div class="row">
      <div class="col-12 col-md-3 offset-md-9">2 comments</div>
    </div>
    <hr>
    <!-- comment form -->
    <form>
      <div class="form-group row no-gutters">
        <label>Leave a comment</label>
        <textarea
          class="form-control"
          placeholder="Enter your comment here ..."
          v-model="newComment"
        ></textarea>
        <button class="btn replybtn" type="submit" @click="postComment">Send</button>
      </div>
    </form>
    <br>
    <!-- sorting by recent -->
    <div class="filter">
      <filterby/>
    </div>
    <!-- reply and posts -->
    <div class="posts">
      <div class="messages">
        <div class="card">
          <div class="card-body" v-for="(comment,n) in comments">
            <p class="card-text comment">{{comment}}</p>
            <button type="button" class="btn btn-sm red" @click="deleteComment(n)">
              <font-awesome-icon class="icon" icon="trash-alt"/>Delete
            </button>
            <button type="button" class="btn btn-sm">
              <font-awesome-icon class="icon" icon="reply"/>Reply
            </button>
          </div>
        </div>
      </div>
      <!-- <posts/> -->
      <reply/>
    </div>
    <hr>
  </div>
</template>

<script>
// import posts from '@/components/posts.vue'
import reply from '@/components/reply.vue'
import filterby from '@/components/filterby.vue'

export default {
  name: 'home',
  components: {
    // posts,
    reply,
    filterby
  },

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
.content {
  background: #fff;
  box-sizing: border-box;
  margin-top: 40px;
  padding: 30px;
}
.replybtn {
  padding: 10px;
  width: 100%;
  background: #314753 !important;
  color: #fff !important;
  float: right;
  margin-top: 10px;
}
textarea {
  width: 50%;
}
.card{
  border: none !important
}
</style>
