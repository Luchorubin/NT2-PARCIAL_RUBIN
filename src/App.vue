<template>
  <div class="container-fluid mt-3">
    <div class="jumbotron">
      <h1><b>Luciano Rubin - Parcial NT2</b></h1>
      <br>

      <Navbar />
      <router-view></router-view>
      
    </div>
  </div>
</template>

<script>

import Navbar from './components/Navbar.vue'

export default {
  name: 'App',
  components: {
    Navbar
  }
}
</script>

<style>
 h1 {
    color: black;
  }
</style>