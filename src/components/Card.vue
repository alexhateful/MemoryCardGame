<template>
  <div class="card" @click="userClick" :class="{matched: match, flipped: !flip}">
    <div :class="{flipped: flip}" class="card-handle">
      <div class="front" :class="{matched: match}">{{cardNumber}}</div>
      <div class="back">&nbsp;</div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
export default defineComponent({
  name: 'Card',
  props: {
    cardNumber: {
      type: Number,
      required: true
    },
    flip: {
      type: Boolean,
      required: true
    },
    position: {
      type: Number,
      required: true
    },
    match: {
      type: Boolean,
      requiered: true
    }
  },
  methods: {
    userClick() {
      if (! this.match ) {
        this.$emit('selected-card', this.position);
      }
    }
  }
})
</script>

<style>
  .card {
    border: 2px solid grey;
    perspective: 1000px;
    transition: ease 0.3s;
  }
  .card.matched {
    opacity: 1 !important;
  }
  .card.flipped {
    opacity: 1 !important;
  }
  .front,
  .back {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
  }
  .front {
    background-color: red;
    color: white;
    font-size: 36px;
    line-height: 100px;
  }
  .front.matched {
    background-color: green;
    transition: ease 0.5s;
    transition-delay: 0.5s;
    animation-duration: 1s;
    animation-name: shake;
    animation-delay: 0.5s;
    animation-play-state: running;
  }
  .back {
    background-color: black;
    transform: rotateY(180deg);
  }
  .card-handle {
    transition: ease-in-out 0.5s transform;
    position: relative;
    height: 100%;
    transform-style: preserve-3d;
  }
  .card-handle.flipped {
    transform: rotateY(180deg);
  }
  
@keyframes shake {
  0% {
    -webkit-transform: scaleX(1);
    transform: scaleX(1)
  }

  10%,
  20% {
    -webkit-transform: scale3d(.9, .9, .9) rotate(-3deg);
    transform: scale3d(.9, .9, .9) rotate(-3deg)
  }

  30%,
  50%,
  70%,
  90% {
    -webkit-transform: scale3d(1.1, 1.1, 1.1) rotate(3deg);
    transform: scale3d(1.1, 1.1, 1.1) rotate(3deg)
  }

  40%,
  60%,
  80% {
    -webkit-transform: scale3d(1.1, 1.1, 1.1) rotate(-3deg);
    transform: scale3d(1.1, 1.1, 1.1) rotate(-3deg)
  }

  to {
    -webkit-transform: scaleX(1);
    transform: scaleX(1)
  }
}

</style>
