<template>
  <div class="content">
    <hr />
    <commentsCount />
    <commentForm />
    <br />
    <sortBy />
    <comments />
    <undoDeleted />
    <hr >
  </div>
</template>

<script>
import commentsCount from '@/components/commentsCount.vue'
import commentForm from '@/components/commentForm.vue'
import sortBy from '@/components/sortBy.vue'
import comments from '@/components/comments.vue'
import undoDeleted from '@/components/undoDeleted.vue'

export default {
  name: 'home',
  components: {
    sortBy,
    commentsCount,
    comments,
    undoDeleted,
    commentForm
  },
}
</script>

<style>
.content {
  background: #fff;
  box-sizing: border-box;
  margin-top: 40px;
  padding: 30px;
}
</style>
