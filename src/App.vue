<template>
  <h1>Memory Cards Game</h1>
  <div class="board" :class="{'not-allowed': lock}">
    <Card :cardNumber="card.number" :flip="card.flip" :position="card.position" :match="card.match" v-for="card in cards" :key="card" @selected-card="selectCard"/>
  </div>
  <p>{{8 - pairs}} pairs left to WIN</p>
  <button @click="StartNewGame">New Game</button>
  <p>
    <label>
      <input type="checkbox" v-model="easyMode">
      easy mode
    </label>
  </p>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import Card from './components/Card.vue'

interface CardItem {
  position: Number,
  number: Number,
  flip: Boolean,
  match: Boolean
}

export default defineComponent({
  name: 'App',
  components: {
    Card
  },
  data() {
    return {
      numbersArr: [0],
      selected: [0 as Number],
      pairs: 0,
      cards: [{} as CardItem],
      timer: 0,
      lock: true,
      easyMode: true
    }
  },
  methods: {
    StartNewGame(): void {
      clearTimeout(this.timer);
      this.pairs = 0;
      this.selected = [];
      this.cards = [];
      this.numbersArr = [1,2,3,4,5,6,7,8];
      this.numbersArr.push(...this.numbersArr);
      for (let i = 0; i < 16; i++) {
        this.cards.push({
          position: i,
          number: this.getNumber(),
          flip: !this.easyMode,
          match: false
        })
      }
      this.timer = setTimeout(() => {
        this.flipBack();
      }, 2000);
    },
    selectCard(num: number): void {
      this.selected.push(this.cards[num].position as Number);
      if (this.selected.length == 2) {
        this.lock = true;
        this.timer = setTimeout(() => {
          this.flipBack();
        }, 2000);
        if (this.cards[this.selected[0] as number].number == this.cards[this.selected[1] as number].number) {
          this.pairs++;
          clearTimeout(this.timer);
          this.cards[this.selected[0] as number].match = true;
          this.cards[this.selected[1] as number].match = true;
        }
        this.selected = [];
      }
      this.cards[num].flip = false;
    },
    getNumber(){
      let randomIndex = Math.floor(Math.random() * this.numbersArr.length);
      let tempArray = [];
      for (let i = 0; i < this.numbersArr.length; i++) {
        if (randomIndex != i) {
          tempArray.push(this.numbersArr[i]);
        }
      }
      let number = this.numbersArr[randomIndex];
      this.numbersArr = tempArray;
      return number;
    },
    flipBack() {
      for (let i = 0; i < this.cards.length; i++) {
        const element = this.cards[i];
        if (! element.match) {
          element.flip = true;
        }
      }
    }
  },
  watch: {
    pairs: function (newValue, oldValue) {
      if (newValue === 8) {
        for (let i = 0; i < this.cards.length; i++) {
          const element = this.cards[i];
          setTimeout(() => {
            element.flip = true;
          }, 100 * i);
        }
        setTimeout(() => {
          alert('you win');
          this.StartNewGame();
        }, 2000);
      }
    },
    timer: function () {
      setTimeout(() => {
        this.flipBack();
        this.lock = false;
      }, 2000);
    }
  },
  beforeMount: function () {
    this.StartNewGame();
  }
})
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.board {
  display: inline-grid;
  grid-template: repeat(4, 100px) / repeat(4, 100px);
  justify-content: center;
  border: 2px solid grey;
}
.board:hover .card:not(:hover) {
  opacity: 0.72;
}
.not-allowed {
  pointer-events: none;
  cursor: not-allowed;
}
</style>
