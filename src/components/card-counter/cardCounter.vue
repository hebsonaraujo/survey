<template>
  <span class="card-counter">{{ index + 1 }} / {{items.length}}</span> 
</template>

<script>
  export default {
    props: ['items','index'],
    data: function () {
      return {
        counter: 0
      }
    }
  }
</script>

<style>
  .card-counter {
    font-weight: bold;
    font-size: 21px;
    display: inline-block;
    background: orange;
    border-radius: 100%;  
    height: 50px;
    width: 50px;
    padding: 10px 5px 10px;
    box-sizing: border-box;
  }

</style>