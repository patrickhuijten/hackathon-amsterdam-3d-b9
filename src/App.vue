<template>
  <div id="app" class="m-0">
    <header />
    <div class="container-fluid p-0">
    <page-home />
    <page-player />
    <page-footer />
    </div>
    <footer>
    </footer>
  </div>
</template>
<style>
html,
body,
#app
{
  margin:0;
  width:100%;
  height:100%;
}

#home,
#footer{
  min-height:100vh;
}
</style>
<script>
import Vue from 'vue';
import PageHome from './pages/home.vue';
import PagePlayer from './pages/player.vue';
import PageFooter from './pages/footer.vue';
Vue.component('page-home', PageHome);
Vue.component('page-player', PagePlayer);
Vue.component('page-footer', PageFooter);
export default {
  name: 'app'
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  overflow-x:hidden;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}
</style>
