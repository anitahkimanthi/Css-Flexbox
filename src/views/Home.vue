<template>
  <div class="row no-gutters justify-content-center">
    <div class="col-12 col-md-6">
      <!-- <parentWrapper/> -->
      <commentBox/>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
// import parentWrapper from '@/components/parentWrapper.vue'
import commentBox from '@/components/commentBox.vue'

export default {
  name: 'home',
  components: {
    // parentWrapper,
    commentBox
  }
}
</script>

<style>
body {
  background: #f7f7f7 !important;
}
</style>
