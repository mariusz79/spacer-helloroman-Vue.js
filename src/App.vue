<template>
  <div class="app">
    <div class="nav">
      <router-link to="/">Search</router-link>
      <router-link to="/about">About</router-link>
    </div>
    <router-view />
  </div>
</template>

<style lang="scss">
@import url('https://fonts.googleapis.com/css?family=Montserrat:300,400,600,800&display=swap');
  *{
    box-sizing: border-box;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }
  body{
    font-family: 'Montserrat', sans-serif;
    padding: 0;
  }
  .nav a{
    margin-left: 10px;
    text-decoration: none;
    color: black;
    font-size: 18px;
  }
</style>
