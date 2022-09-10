<template>
  <button 
    v-bind:disabled="btnDisabled"
    id="btn-pass" 
    type="button" 
    class="btn btn-primary" 
    >
      Próximo
  </button>
</template>

<script>
  export default {
    props: ['btnDisabled'],
    data: function () {
      return {
        counter: 0
      }
    }   
  }

</script>