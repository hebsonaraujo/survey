<template>
  <form ref="surveyForm">
    <div ref="questions" v-for="(alternative,index) in item.alternatives" class="card-questions"
    :class=" alternative.checked ? 'checked' : 'none'" v-bind:key="index">
    
    <label :for="index">
      <input @click.self="initValidation(item.type, $event)" :type="item.type" :id="index" name="questions"
      :value="alternative.text" />
      {{alternative.text}}
    </label>
    
  </div>
</form>
</template>

<script>
  import Vue from 'vue';
  export default {
    props: ['item','currentActivePosition'],
    data(){
      return {
        btnDisabled: true
      }
    },
    methods: {
      radioExec: function (ev) {        
        this.radioSelected(ev);        
      },
      checkboxExec: function (ev) {
        this.checkSelected(ev);
      },
      checkSelected: function ({ target: { id } }) {
        const { alternatives } = this.item;
        if (alternatives[id].checked) {
          alternatives[id].checked = false;
        }
        else {
          this.btnDisabled = false;
          Vue.set(alternatives, id, { ...this.item.alternatives[id], "checked": true });
        }
        this.btnDisabled = !alternatives.some(el => el.checked);
        this.$emit('custom',this.btnDisabled);
      },
      initValidation: function (inputType, ev) {
        this[`${inputType}Exec`](ev);
      },
      radioSelected: function ({ target: { id } }) {
        this.btnDisabled = false;
        const { alternatives } = this.item;
        alternatives.forEach(el => {
          if (el.checked) {
            el.checked = false;
          }
        });
        this.$emit('custom',this.btnDisabled);
        Vue.set(alternatives, id, { ...this.item.alternatives[id], "checked": true });
      }
    }
    
  }
</script>

<style>
  label {
    padding: 20px 48px 20px 90px;
    display: block;
    position: relative;
    padding: 10px 10% 10px calc(10% + 35px);
    cursor: pointer;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
  }
  .card-questions:hover {
    background-color: rgba(255, 255, 255, 0.4);
  }
  .card-questions.checked {
    background-color: #7d461a;
  }
  
</style>