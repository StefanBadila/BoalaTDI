<template>
 <h1>BoalaTDI</h1>
<br>
<section class="game-board">
  <Card 
  v-for="(card,index) in cardList" 
  :key="'card-${index}'"
  :matched="card.matched"
  :value="card.value"
  :visible="card.visible"
  :position="card.position"
  @select-card="flipCard"
/>
</section>
<br>
<h4>{{status}}</h4>
<br>
<button @click="restartGame">Amestec Tiganesc</button>
</template>

<script>
import _ from 'lodash'
import { computed , ref, watch } from 'vue'
import Card from './components/Card.vue'

export default{
  name: 'App',
  components:{
    Card
  },
  setup () {
    const cardList = ref([])
    const userSelection =ref([])

    const status= computed (() => {
      if(remainingPairs.value === 0){
        return `Player wins!`     }
         else {
         return `Remaining Pairs: ${remainingPairs.value}`
      }
    })

    const remainingPairs = computed(() => {
      const remainingCards= cardList.value.filter
      (card => card.matched === false).length
      
    return remainingCards / 2
    })

    const shuffleCards = () => {
      cardList.value = _.shuffle(cardList.value)
    }

    const restartGame = () => {
      shuffleCards()
      cardList.value = cardList.value.map((card, index) => {
        return{
          ...card,
          matched: false,
          position: index,
          visible: false
        }
      })

    }

    const cardItems = [
    'vw',
    'dacia',
    'audi',
    'bmw',
    'mercedes',
    'tesla',
    'mazda',
    'lambo'
    ]

    cardItems.forEach(item => {
      cardList.value.push({
        value: item,
        visible: false,
        position: null,
        matched: false
      })
      cardList.value.push({
        value: item,
        visible: false,
        position: null,
        matched: false
      })

    })

    cardList.value = cardList.value.map((card,index)=> {
      return{
        ...card,
        position: index
      }
    })
    
    const flipCard = payload => {
     
      cardList.value[payload.position].visible = true;

      if(userSelection.value[0]){
        if ((userSelection.value[0].position === payload.position) && (userSelection.value[0].faceValue === payload.faceValue) ){
        return
      }else{
        userSelection.value[1]=payload
      }
      } else {
        userSelection.value[0]=payload
      }
    }
    watch(userSelection, 
    currentValue => {
      if(currentValue.length === 2){
        const cardOne = currentValue[0];
        const cardTwo = currentValue[1];

        

        if(cardOne.faceValue === cardTwo.faceValue){
            cardList.value[cardOne.position].matched=true;
            cardList.value[cardTwo.position].matched=true;
        }
        else{

          setTimeout(() => {
            cardList.value[cardOne.position].visible=false;
            cardList.value[cardTwo.position].visible=false;
          },750)
        
        }


        userSelection.value.length = 0
      }

    },
    { deep: true }
    )

    return{
      cardList,
      flipCard,
      userSelection,
      status,
      shuffleCards,
      restartGame
    }
  }
} 
</script>

<style>

html, body { 

  
  margin: 0;
  padding: 0;
  top:50%;
  left:50%;
  transform: translate(-50%,-50%);
  position: absolute;
  justify-content: center;
  background-image: url('/imagini/anvelope-all-season.jpg');
}

h1 {
  margin-top: 0;
  padding-top: 20%;
}

#app {
font-family: Arial, Helvetica, sans-serif;
-webkit-font-smoothing: antialiased;
-moz-osx-font-smotthing: grayscale;
text-align: center;
color: #ffffff;
background-image: url('/imagini/anvelope-all-season.jpg');
height: 100vh;
}

.game-board{
  display: grid;
  grid-template-columns: repeat(4, 120px);
  grid-template-rows: repeat(4, 120px);
  grid-column-gap: 24px;
  grid-row-gap: 24px;
}

</style>
