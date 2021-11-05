<template>
  <div class="container">
    <router-view />
  </div>
</template>

<style>
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

.container {
  margin-top: 30px;
  margin-bottom: 30px;
  max-width: 1150px;
  margin-right: auto;
  margin-left: auto;
}
</style>
