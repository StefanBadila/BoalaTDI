<template>
<h1 class="sr-only">Boala TDI</h1>
  <div class='title' style="text-align: center;">
    <img src="/imagini/Boala-TDI.png" alt="Boala-TDI" class="title">
  </div>
<br>
<transition-group tag="section" class="game-board" name="shuffle-card-move">
  <Card 
  v-for="card in cardList" 
  :key="`${card.value}-${card.variant}`"
  :matched="card.matched"
  :value="card.value"
  :visible="card.visible"
  :position="card.position"
  @select-card="flipCard"
/>
</transition-group>
<br>
<button v-if="newPlayer" @click="startGame" class="button"><img src="/imagini/restart.svg" 
  alt="Restart Icon "/>Incepe jocul</button>
<button v-else @click="restartGame" class="button"><img src="/imagini/restart.svg" 
  alt="Restart Icon "/>Reseteaza</button>
</template>

<script>
import _ from 'lodash'
import { computed , ref, watch } from 'vue'
import {launchConfetti} from './utilities/confetti'
import Card from './components/Card.vue'


export default{
  name: 'App',
  components:{
    Card
  },
  setup () {
    const cardList = ref([])
    const userSelection =ref([])
    const newPlayer = ref(true)

    const startGame = () => {

      newPlayer.value= false
      restartGame()
    }


    const remainingPairs = computed(() => {
      const remainingCards= cardList.value.filter
      (card => card.matched === false).length
      
    return remainingCards / 2
    })


    const restartGame = () => {
      cardList.value = _.shuffle(cardList.value)
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
    'volvo',
    'lambo'
    ]

    cardItems.forEach(item => {
      cardList.value.push({
        value: item,
        variant: 1,
        visible: true,
        position: null,
        matched: false
      })
      cardList.value.push({
        value: item,
        variant: 2,
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

    watch(remainingPairs, currentValue =>{
      if(currentValue === 0)
        launchConfetti()
    })

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
      restartGame,
      newPlayer,
      startGame
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


#app {
font-family: Arial, Helvetica, sans-serif;
-webkit-font-smoothing: antialiased;
-moz-osx-font-smotthing: grayscale;
text-align: center;
color: #ffffff;
background-image: url('/imagini/anvelope-all-season.jpg');
height: 100vh;
}

.button{ 
background-color: rgba(13, 197, 194, 0.457);
color: white;
padding: 0.75rem 0.5rem;
display: flex;
align-items: center;
justify-content: center;
margin: 0 auto;
margin-bottom: 20%;
font-weight: bold;
font-size: 1.2rem;
border: 0;
border-radius: 15%;
}


.button img{
  padding-right: 5px;
}

.game-board{
  display: grid;
  grid-template-columns: repeat(4, 120px);
  grid-template-rows: repeat(4, 120px);
  grid-column-gap: 24px;
  grid-row-gap: 24px;
  padding-bottom: 3%;
}


.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  border: 0;
}

.title { 
    padding-top: 5%;
    padding-bottom: 2%;
}

.shuffle-card-move{
  transition:transform 0.8s ease-in;
}
</style>
