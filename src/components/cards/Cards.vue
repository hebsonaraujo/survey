<template>
  <section class="cards">
    <div v-for="(item,index) in items" v-if="item.active">
      <!-- do not use v-if with v-for https://v2.vuejs.org/v2/style-guide/#Avoid-v-if-with-v-for-essential -->

      <CardHeader></CardHeader>
      <CardCounter :items="items" :index="index"></CardCounter>
      <section class="card-wrapper">        
        <Title :item="item"></Title>
        <Form @custom="updateBtn" :item="item" :currentActivePosition="currentActivePosition"></Form>
      </section>
    </div>
    
    <Button @click.native="btnNext" :btnDisabled="btnDisabled" ></Button>
  </section>
</template>

<script>
import Vue from 'vue';
import CardHeader from '../card-header/CardHeader.vue';
import myData from '../../../../../data.js';
import CardCounter from '../card-counter/cardCounter.vue';
import Button from '../button/Button.vue';
import Title from '../title/Title.vue';
import Form from '../form/Form.vue';

export default {
    components: { CardHeader, CardCounter, Button, Title, Form },
    data() {
        return {
            items: myData,
            btnDisabled: true,
            currentActivePosition: 0,
            itemsLength: myData.length,
            storeAnswers: [],
            hasANextQuestion: () => this.currentActivePosition < this.itemsLength,
            checked: false
        };
    },
    methods: {
        /**
         * TODO
         *
         * 3 armazenar os dados corretos referentes a cada questao
         * 4. enviar informacoes das questoes suas respotas corretas
         */
        setStatusCard: function (index, status) {
            Vue.set(this.items, index, { ...this.items[index], "active": status });
        },
        btnNext: function () {
            if (this.hasANextQuestion) {
                this.setStatusCard(this.currentActivePosition, false);
                this.setStatusCard(++this.currentActivePosition, true);
                this.btnDisabled = !this.btnDisabled;
            }
        },
        updateBtn: function(btnStatus){
          this.btnDisabled = btnStatus;
          console.log('FFF',btnStatus)        
        }

        
        
    },
    mounted: function () {
        this.setStatusCard(0, true);
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
</style>
