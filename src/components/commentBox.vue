<template>
  <div class="content">
    <div class="row">
      <!-- comments count -->

      <div class="col-12 col-md-3 offset-md-9">
        <font-awesome-icon icon="comments" v-model="allComments"/>
         Comments ({{ comments.length }})
      </div>
    </div>
    <hr>

    <!-- comment form -->
    <form  v-on:submit.prevent="onSubmit">
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
      <!-- <sortBy /> -->
      <button class="btn btn-sm sort" @click="reverse">
        sort by latest
        <font-awesome-icon
          class="icon sorticon"
          icon="angle-down"
          :key="expand_more"
          @click="reverse"
        />
      </button>
    </div>
    <br>
    <!-- comments and replies-->
    <div class="posts">
      <div class="messages">
        <div class="card">
          <div
            class="card-body"
            v-for="(comment, index) in comments"
            v-bind:key="index"
          >
            <p class="card-text comment">
              <b>person {{ comment.person }}</b>  - <span class="time" >{{ comment.datetime }}</span> <br />
              <br />{{ comment.message }}
            </p>
            <button
              type="button"
              class="btn btn-sm red"
              @click="deleteComment(index)"
            >
              <font-awesome-icon class="icon" icon="trash-alt"/><span>Delete</span> 
            </button>

            <button
              type="button"
              class="btn btn-sm"
              v-if="{ active: activeIndex === index }"
              :class="{ active: activeIndex === index }"
              @click="toogleReply(index)"
              :key="index"
            >
              <font-awesome-icon class="icon" icon="reply" else/>
              <span>Reply {{ replies.length }}</span>
            </button>
            <hr>
          </div>
        </div>

        <!-- reply to comments -->
        <div class="col-11 offset-md-1" v-show="OpenReply">

          <div
            class="card"
            v-for="(reply, index) in replies"
            v-bind:key="index"
          >
            <div class="card-body">
              <p class="card-text">{{ reply.message }}</p>
              <button
                type="button"
                class="btn btn-sm"
                @click="deleteReply(index)"
              >
                <font-awesome-icon class="icon" icon="trash-alt"/> <span>Delete</span>
              </button>
            </div>
          </div>
          <br>
          <form  v-on:submit.prevent="onSubmit" class="card-body">
            <div class="form-group row no-gutters">
              <textarea
                class="form-control"
                placeholder="Reply here ..."
                v-model="newReply"
              ></textarea>
              <button class="btn post" type="submit" @click="postReply">Reply</button>
            </div>
          </form>
        </div>
      </div>
    </div>

    <!-- undo the changes -->
    <div
      class="col-12 col-md-3 offset-md-9"
      @click="undo()"
      :disabled="!canUndo"
    >
      Undo changes
    </div>
  </div>
</template>

<script>
// import sortBy from '@/components/sortBy.vue'

export default {
  name: 'home',
  components: {
    // sortBy
  },

  data() {
    return {
      comments: [],
      newCommentId: 1,
      person:1,
      replies: [],
      newReplyId: 1,
      OpenReply: false,
      datatime:null
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
    reverse() {
      this.comments.reverse()
    },
    // commentbox methods and daata storage
    postComment() {
      if (!this.newComment) {
        return
      }

      this.comments.push({
        id: this.newCommentId++,
        message: this.newComment,
        person: this.person++,
        datetime: this.datatime = Date().format()
      })
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
      this.replies.push({
        id: this.newReplyId++,
        message: this.newReply
      })
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
      ;(this.OpenReply = !this.OpenReply), (this.activeIndex = index)
    },

    allComments() {
      return parseInt(this.comments.length) + parseInt(this.replies.length)
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
.active {
  font-weight: 900 !important;
  color: #47b6cf !important;
}
.replybtn {
  padding: 10px;
  width: 100%;
  background: #47b6cf !important;
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
  font-size: 11px;
  color: #8f8d8d;
}
.post {
  background: #47b6cf !important;
  width: 100%;
  color: #fff !important;
  margin-top: 20px;
}
.sort {
  border: 1px solid #ccc !important;
}
.sorticon {
  margin-left: 10px;
}
span {
  font-size: 11px;
  color: #8f8d8d;
}
.comment{
  font-size: 14px;
}
.card-body{
  padding:0 !important;
}
</style>