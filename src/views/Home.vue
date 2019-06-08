<template>
  <div class="row no-gutters justify-content-center">
    <div class="col-12 col-md-6">
      <CommentForm />
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import CommentForm from '@/components/CommentForm.vue'

export default {
  name: 'home',
  components: {
    CommentForm
  }
}
</script>

<style>
body {
  background: #ccc !important;
}

</style>
