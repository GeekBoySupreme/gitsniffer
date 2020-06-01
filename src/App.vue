<template>
  <div id="app">
    <GitSniffer/>
    <Footer/>
  </div>
</template>

<script>
import GitSniffer from './components/GitSniffer.vue'
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
</style>

