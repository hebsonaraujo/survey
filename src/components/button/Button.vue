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
    props: ['btnDisabled']
  }

</script>
<style>
  #btn-pass[disabled] {
    background-color: rgba(0, 0, 0, 0.5);
    color: #666;
    cursor: not-allowed;
}

#btn-pass {
  display: block;
    background-color: #7d461a;
    height: 48px;
    padding: 0px 30px;
    color: #fff;
    font-family: "Gilroy";
    font-weight: 800;
    font-size: 1.4em;
    border: 0;
    border-radius: 24px;
    cursor: pointer;
    margin: 0 10%;
}
</style>