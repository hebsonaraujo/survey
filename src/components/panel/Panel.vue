<template>
  <section class="panel">
    <card-survey></card-survey>
  </section>
</template>

<script>
import Cards from '../cards/Cards.vue'
export default {  
  components: {    
    'card-survey': Cards
  }
}
</script>

<style>
.panel {
  background: url('http://www.panificadorakennedy.com.br/pesquisa/assets/imgs/bg.jpg') no-repeat center center fixed;
    background-size: cover;
    min-height: 100%;
    display: grid;
    align-items: center;
    justify-items: center;
}
</style>
