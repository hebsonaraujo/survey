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
            v-for="(k,index) in item.alternatives"                         
            class="card-questions "
            :class=" k.checked ? 'checked' : 'none'"
            v-bind:key="index" >

            <label :for="index"> <!--$refs.questions[index].classList.add('checked')-->
              <!-- @click.self="itemSelected item.type === 'radio'? itemSelected : -->
              <input  @click.self="initValidation(item.type,$event)" 
                :type="item.type" 
                :id="index" name="vehicle1" value="Bike"  />
               {{k.text}}
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
      hasActive: false,
      btnDisabled: true,
      currentActivePosition: 0,
      itemsLength: myData.length,
      storeAnswers: [],   
      radioTypeMethods: () => console.log('RADIO')   
    }
  },
  
  methods: {
    setStatusCard: function(index,status) {      
      Vue.set(
        this.items,
        index,
        { ...this.items[index],'active': status}
      )   
    },
    btnNext: function(ev) {
      if( this.currentActivePosition < this.itemsLength) { 
        this.setStatusCard(this.currentActivePosition,false)
        this.setStatusCard(++this.currentActivePosition,true)
        this.btnDisabled = !this.btnDisabled
      }      
    },
    radioExec: function(la) {
      this.itemSelected(la)
    },
    checkboxExec: function() {
      alert('lullu')

    },
    initValidation: function(ev,la) {
      this[`${ev}Exec`](la);     
    },
    itemSelected: function({ target: { id } }) {      
      this.btnDisabled = false;
      const { alternatives } = this.items[0];
      alternatives.forEach( el => {
        if(el.checked) {
          el.checked = false
        }        
      })     
      
      Vue.set(
        this.items[0].alternatives,
        id,
        { text:this.items[0].alternatives[id].text,'checked': true}
      )
      
      // this.$forceUpdate();
      // this.$refs.questions[id].classList.add('checked');
      // console.log('@@@@',this.lala(),id,this.items)//id,this.$refs,ev.target.id)
      // console.log('---',ev,ev.target.parentElement.parentElement.classList.add('checked')) //this,this.$refs,this.$refs.surveyForm[0], v-on:click.capture="addCheck"
    }


  },
  mounted: function () {
    this.setStatusCard(0,true)
    
    console.log('innn',this.radioTypeMethods(),myData,this.itemsLength)  
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
