<script>
import {computed} from 'vue'
export default{
props:{
    matched: {
        type: Boolean,
        default: false
    },
    position: {
        type: Number,
        required: true
    },
    value: {
        type: String,
        required: true
    },

visible: { 
    type: Boolean,
    default: false
    }
},
setup(props, context) {
    const flippedStyles = computed(() => {
        if(props.visible) {
            return 'is-flipped'
        }
    })

    const selectCard = () => {
        context.emit('select-card', {
            position: props.position,
            faceValue: props.value
        })
    }
    return {
        flippedStyles,
        selectCard
        
    }
}
}
</script>

<template>
<div class="card" :class="flippedStyles" @click="selectCard">
        <div class="card-face is-front">
        <img :src="`/imagini/${value}.png`" 
        :alt="value" />
        <img v-if="matched" src="/imagini/checkmark.svg" 
            class="icon-checkmark"/>
    </div> 
    <div class="card-face is-back"></div>
</div>
</template>

<style>
.card {
    position: relative;
    transition: 0.5s transform ease-in;
    transform-style: preserve-3d;
  }
 
  .card.is-flipped{
    transform: rotateY(180deg)
  }

  .card-face {
    width: 100%;
    height: 100%;
    position: absolute;
    border-radius: 10px;
    display: flex;
    align-content: center;
    justify-content: center;
    backface-visibility: hidden;
  }

  .card-face.is-front {
    background-image: url('/imagini/carduri-fata.png');
    color: white;
    transform: rotateY(180deg);
  }

  .card-face.is-back {
    background-image:  url('/imagini/cauciucuri-final.png');
    background-repeat: no-repeat;
    background-position: center;
    color: white;
   
  }
 

.icon-checkmark{
        position: absolute;
        right: 5px;
        bottom: 5px;

}

  </style>