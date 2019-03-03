<template>
  <div id="app">
    <!-- <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div> -->
    <router-view/>
  </div>
</template>
<style lang="stylus">
html, body
  padding 0
  margin 0
#app
  font-family Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color black
  margin-top 10px

#nav
  padding 30px
  a
    font-weight bold
    color black
    &.router-link-exact-active
      color blue
</style>
