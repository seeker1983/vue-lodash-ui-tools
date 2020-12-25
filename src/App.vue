<template>
  <div id="app">
    <h2> Live Links Diff Tools </h2>
    <div id="nav" v-show=0>
      <router-link to="/">Lodash Converter</router-link> |
      <router-link to="/about">Info</router-link> |
      <div id=logo-container><img alt="Vue logo" width=80 src="./assets/logo.png"></div>
    </div>
    <router-view/>
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}

#logo-container {
  display:inline;
  vertical-align: sub;
}
</style>
