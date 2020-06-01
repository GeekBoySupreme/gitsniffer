<template>
  <div id="app">
    
    <Footer/>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'

export default {
  mounted() {},
  name: 'App',
  components: {
    GitSniffer,
    Footer
  }
}
</script>

<style>
  @import './assets/styles.css';
  @import 'https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/4.4.0/css/bootstrap-grid.css';
</style>

