<template>
  <div class="content">
    <div class="row">
      <div class="col-12 col-md-3 offset-md-9">
        <b>{{comments.length}} comments</b>
      </div>
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
          @keyup.enter="postComment"
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
          <div class="card-body" v-for="(comment, n) in comments" v-bind:key="comment">
            <p class="card-text comment">{{ comment }}</p>
            <button type="button" class="btn btn-sm red" @click="deleteComment(index)">
              <font-awesome-icon class="icon" icon="trash-alt"/>Delete
            </button>

            <button type="button" class="btn btn-sm" @click="toogleReply">
              <font-awesome-icon class="icon" icon="reply"/>
              Reply {{replies.length}}
            </button>
          </div>
        </div>
      </div>

      <!-- reply comment box -->

      <div class="reply">
        <div class="col-11 offset-md-1">
          <div class="card" v-for="(reply, n) in replies" v-bind:key="reply">
            <div class="card-body">
              <p class="card-text">
                {{reply}}
              </p>
              <button type="button" class="btn btn-sm" @click="deleteReply(index)">
                <font-awesome-icon class="icon" icon="trash-alt"/>Delete
              </button>
            </div>
          </div>
        </div>
      </div>
      <!-- reply to the comment end -->

      <!-- message in put for reply -->
      <form class="col-6 offset-md-1" v-show="OpenReply">
        <div class="form-group row no-gutters">
          <textarea
            class="form-control"
            placeholder="Reply here ..."
            @keyup.enter="postReply"
            v-model="newReply"
          ></textarea>
          <button class="btn post" type="submit" @click="postReply">Post</button>
        </div>
      </form>
    </div>
    <hr>
    <div class="col-12 col-md-3 offset-md-9" @click="undo()" :disabled="!canUndo">Undo changes</div>
  </div>
</template>

<script>
import filterby from '@/components/filterby.vue'

export default {
  name: 'home',
  components: {
    filterby
  },

  data() {
    return {
      comments: [{}],
      replies: [{}],
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

    toogleReply() {
      this.OpenReply = !this.OpenReply
    }
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
.card {
  border: none !important;
}
.icon {
  margin-right: 10px !important;
}
.post {
  background: #314753 !important;
  width: 100%;
  color: #fff !important;
  margin-top: 20px;
}
</style>
