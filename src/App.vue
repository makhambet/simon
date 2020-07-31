<template>
  <div id="app">
    <simon-block></simon-block>
  </div>
</template>

<script>
import SimonBlock from './components/SimonBlock.vue'

export default {
  name: 'App',
  components: {
    SimonBlock
  }
}
</script>

<style>
  body {
    margin: 0;
    padding: 0;
  }

</style>
