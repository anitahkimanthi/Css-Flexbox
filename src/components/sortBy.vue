<template>
  <div class="row no-gutters">
    <div class="form-group col-12 col-md-3">
      <select id="inputState" class="form-control" data-role="sort">
        <option selected>sort by recent</option>
        <option>sort by Oldest</option>
      </select>
    </div>
  </div>
</template>
<script>
export default {
  name: 'sortBy'
}
</script>
