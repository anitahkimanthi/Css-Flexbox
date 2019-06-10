<template>
  <div class="content">
    <div class="row">
      <!-- comments count -->
    
      <div class="col-12 col-md-3 offset-md-9">
        <font-awesome-icon class="icon" icon="comments" v-model="allComments"/>
        {{ comments.length}}
      </div>
    </div>
    <hr>

    <!-- comment form -->
    <form method="POST" v-on:submit.prevent="postComment">
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
        <button class="btn replybtn" type="submit" @click="postComment">Send</button>
      </div>
    </form>
    <br>

    <!-- sorting by recent -->
    <div class="filter">
      <sortBy/>
    </div>

    <!-- comments -->
    <div class="posts">
      <div class="messages">
        <div class="card">
          <div class="card-body"  v-for="(comment, index) in comments" v-bind:key="index">
            <p class="card-text comment">{{ comment }}</p>
            <button type="button" class="btn btn-sm red" @click="deleteComment(index)">
              <font-awesome-icon class="icon" icon="trash-alt"/>Delete
            </button>

            <button type="button" class="btn btn-sm" :class="{ 'active': activeIndex === index}"  @click="toogleReply(index)" :key="index">
              <font-awesome-icon class="icon" icon="reply"/>
              Reply {{ replies.length }}
            </button>
          </div>
        </div>

        <div class="col-11 offset-md-1" v-show="OpenReply">
          <div class="card" v-for="(reply, index) in replies" v-bind:key="index">
            <div class="card-body">
              <p class="card-text">{{ reply }}</p>
              <button type="button" class="btn btn-sm" @click="deleteReply(index, 1)">
                <font-awesome-icon class="icon" icon="trash-alt"/>Delete
              </button>
            </div>
          </div>
          <form class="card-body">
            <div class="form-group row no-gutters">
              <textarea class="form-control" placeholder="Reply here ..." v-model="newReply"></textarea>
              <button class="btn post" type="submit" @click="postReply">Post</button>
            </div>
          </form>
        </div>
      </div>

      <!-- replies-->

      <div class="reply"></div>
      <!-- reply to the comment end -->

      <!-- reply box-->
    </div>
    <hr>

    <!-- undo the changes -->
    <div class="col-12 col-md-3 offset-md-9" @click="undo()" :disabled="!canUndo">Undo changes</div>
  </div>
</template>

<script>
import sortBy from '@/components/sortBy.vue'

export default {
  name: 'home',
  components: {
    sortBy
  },

  data() {
    return {
      comments: [],
      replies: [],
      newComment: null,
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
    if (this.comments.length) {
      this.comment = this.comments[0];
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

    // reply the comments  functions

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

    toogleReply(index) {
      this.OpenReply = !this.OpenReply,
       this.activeIndex = index
    },

    allComments(){
          return parseInt(this.comments.length) + parseInt(this.replies.length);
    },
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
.active {
  font-weight: 700 !important;
  color:#47B6CF !important;
}
.replybtn {
  padding: 10px;
  width: 100%;
  background: #47B6CF !important;
  color: #fff !important;
  float: right;
  margin-top: 10px;
}
textarea {
  width: 50%;
}
.card {
  border: none !important;
}
.icon {
  margin-right: 10px !important;
}
.post {
  background: #47B6CF !important;
  width: 100%;
  color: #fff !important;
  margin-top: 20px;
}
</style>