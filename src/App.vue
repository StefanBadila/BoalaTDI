<template>
 <h1>BoalaTDI</h1>
<br>
<section class="game-board">
  <Card v-for="(card,index) in cardList" 
  :key="'card-${index}'"
  :value="card"/>
</section>
</template>

<script>
import Card from './components/Card.vue'

export default{
  name: 'App',
  components:{
    Card
  },
  setup () {
    const cardList = []

    for (let i=0; i<16;i++){
      cardList.push(i)
    }
    return{
      cardList
    }
  }
}
</script>

<style>
#app {
font-family: Arial, Helvetica, sans-serif;
-webkit-font-smoothing: antialiased;
-moz-osx-font-smotthing: grayscale;
text-align: center;
color: black;
margin-top: 60px;
}

.card {
  border: 5px solid #ccc;
}

.game-board{
  display:grid;
  grid-template-columns: 100px 100px 100px 100px;
  grid-template-rows: 100px 100px 100px 100px;
  grid-column-gap: 30px;
  grid-row-gap: 30px;
  justify-content: center;
}
</style>
