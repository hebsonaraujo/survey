<template>
  <section class="cards" >
    <div v-for="(item,index) in items" v-if="item.active">
      <!-- do not use v-if with v-for https://v2.vuejs.org/v2/style-guide/#Avoid-v-if-with-v-for-essential -->

      <header>
        <span id="progress">{{ index + 1 }} / {{items.length}}</span>  
        <img src="http://www.panificadorakennedy.com.br/pesquisa/assets/imgs/logo.png">
      </header>

      <section class="card-wrapper" >
        <h2 id="pergunta"> {{item.question}} </h2>

        <form  ref="surveyForm">
          <div          
            ref="questions"
            v-for="(alternative,index) in item.alternatives"                         
            class="card-questions "
            :class=" alternative.checked ? 'checked' : 'none'"
            v-bind:key="index" >

            <label :for="index">
              <input  @click.self="initValidation(item.type, $event)" 
                :type="item.type" 
                :id="index" 
                name="questions"
                :value="alternative.text" />
               {{alternative.text}}
            </label>

          </div>          
        </form>

      </section>

    </div>    
    
    <button 
      @click = "btnNext"
      v-bind:disabled="btnDisabled"
      id="btn-pass" 
      type="button" 
      class="btn btn-primary" 
      >
        Próximo
    </button>
  </section>
</template>

<script>
import Vue from 'vue';
import myData from '../../../../../data.js'

export default {  
  data () {  
    return {      
      items: myData,      
      btnDisabled: true,
      currentActivePosition: 0,
      itemsLength: myData.length,
      storeAnswers: [],
      hasANextQuestion: () => this.currentActivePosition < this.itemsLength,      
      checked: false  
    }
  },  
  methods: {
    /**
     * TODO     
     * 
     * 3 armazenar os dados corretos referentes a cada questao
     * 4. enviar informacoes das questoes suas respotas corretas
     */
    setStatusCard: function(index,status) {      
      Vue.set(
        this.items,
        index,
        { ...this.items[index],'active': status}
      )   
    },
    btnNext: function(ev) {
      if( this.hasANextQuestion ) { 
        this.setStatusCard(this.currentActivePosition,false)
        this.setStatusCard(++this.currentActivePosition,true)
        this.btnDisabled = !this.btnDisabled
      }      
    },
    radioExec: function(ev) {
      this.itemSelected(ev)
    },
    checkboxExec: function(ev) {
      this.checkSelected(ev)
    },
    checkSelected: function({ target: { id } }) {
      const { alternatives } = this.items[this.currentActivePosition];      
      if( alternatives[id].checked ) {
        alternatives[id].checked  = false;        
      } else {        
        this.btnDisabled = false
        Vue.set(
          alternatives,
          id,
          { ...this.items[this.currentActivePosition].alternatives[id],'checked': true}
        )
      }
      this.btnDisabled = !alternatives.some(el => el.checked);
    },
    initValidation: function(inputType,ev) {
      this[`${inputType}Exec`](ev);
    },
    itemSelected: function({ target: { id } }) {      
      this.btnDisabled = false;
      const { alternatives } = this.items[this.currentActivePosition];
      alternatives.forEach( el => {
        if(el.checked) {
          el.checked = false
        }
      }) 
      
      
      Vue.set(
        alternatives,
        id,
        { ...this.items[this.currentActivePosition].alternatives[id],'checked': true}
      )
      
    }


  },
  mounted: function () {
    this.setStatusCard(0,true)
  }
}
</script>

<style>
.cards {
    background: rgba(0, 0, 0, 0.9);
    color: white;
    border-radius: 10px;
    padding: 20px 40px;
    margin-top: 30px;
    width: 600px;
}
.cards li {
  font-size: 18px;
  padding: 10px;
}
.cards ul li:before {  
    border: 2px solid white;
    content: '';
    width: 13px;
    height: 13px;
    margin: 0 15px;
    border-radius: 100%;
    display: inline-block;
}
.cards ul li:hover {
  color: black;
  background-color:rgba(256, 256, 256, 0.6);
  border-radius: 10px;
  cursor: pointer;
}
.cards span {
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
.cards h2 {
  font-weight: bold;
  text-align: center;
  margin: 15px 0;
}

.cards ul {
  list-style: none;
}
.card-wrapper {

}
.card-questions label {
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
