<template>
  <div id="app">
    <div class="row no-gutters">
      <div class="col-12 header text-center">
        <h2>Vue CommentBox</h2>
      </div>
    </div>
    <router-view />
  </div>
</template>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
.header {
  background: #47B6CF;
  color:#fff !important;
  text-align: center;
  padding: 30px;
}
.btn{
  color:#405d6d !important;
}
</style>
