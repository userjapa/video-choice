<template>
  <div id="app" class="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view class="app__content"/>
  </div>
</template>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  border: none;
}

html, body {
  height: 100vh;
  width: 100vw;
}

.app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height: 100vh;
  &__content {
    height: calc(100vh - #{78px});
    max-height: calc(100vh - #{78px});
  }
}

#nav {
  padding: 30px;
  a {
    font-weight: bold;
    color: #2c3e50;
    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
