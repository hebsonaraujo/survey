<template>
  <h2 class="title"> {{item.question}} </h2>
</template>

<script>
  export default {
    props: ['item'],
    data: function () {
      return {
        
      }
    }   
  }

</script>

<style>

.title {
  font-weight: bold;
  text-align: center;
  margin: 15px 0;
}
</style>