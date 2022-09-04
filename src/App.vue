<template>
  <div id="app">
    <panel-background></panel-background>
  </div>
</template>

<script>
import Panel from './components/panel/Panel.vue'
export default {  
  components: {    
    'panel-background': Panel
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
}
</script>

<style>

</style>
