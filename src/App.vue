<template>
  <div id="nav">
    <router-link to="/">倒计时 </router-link> <span> | </span>
    <router-link to="/about"> 关于</router-link>
  </div>
  <router-view/>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  min-height: 100vh;
  font-size: 12px;
  /* background-image: radial-gradient(#515a5f, #121317); */
}

#nav {
  padding: 30px 0 0 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
