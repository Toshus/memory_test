<template>
  <div class="game-wrapper">
    <game-header :scores="scores"></game-header>
    <div class="cards">
      <div
        class="card"
        v-for="(card, index) in cards"
        :key="index"
        :class="{ flipped: card.flipped, found: card.found }"
        @click="flipCard(card)"
      >
        <div class="back"></div>
        <div class="front" :style="{ backgroundImage: 'url(' + card.image + ')' }"></div>
      </div>
    </div>
    <button @click="shuffleCards">Shuffle</button>
  </div>
</template>

<script>
import Header from './Header'

export default {
  name: 'GameScreen',
  components: {
    'game-header': Header
  },
  data () {
    return {
      scores: 0,
      cards: [
        { name: 'c01', image: '/static/img/c01.png', flipped: false, found: false },
        { name: 'c02', image: '/static/img/c02.png', flipped: false, found: false },
        { name: 'c03', image: '/static/img/c03.png', flipped: false, found: false },
        { name: 'c04', image: '/static/img/c04.png', flipped: false, found: false },
        { name: 'c05', image: '/static/img/c05.png', flipped: false, found: false },
        { name: 'c06', image: '/static/img/c06.png', flipped: false, found: false },
        { name: 'c07', image: '/static/img/c07.png', flipped: false, found: false },
        { name: 'c08', image: '/static/img/c08.png', flipped: false, found: false },
        { name: 'c09', image: '/static/img/c09.png', flipped: false, found: false },
        { name: 'c01', image: '/static/img/c01.png', flipped: false, found: false },
        { name: 'c02', image: '/static/img/c02.png', flipped: false, found: false },
        { name: 'c03', image: '/static/img/c03.png', flipped: false, found: false },
        { name: 'c04', image: '/static/img/c04.png', flipped: false, found: false },
        { name: 'c05', image: '/static/img/c05.png', flipped: false, found: false },
        { name: 'c06', image: '/static/img/c06.png', flipped: false, found: false },
        { name: 'c07', image: '/static/img/c07.png', flipped: false, found: false },
        { name: 'c08', image: '/static/img/c08.png', flipped: false, found: false },
        { name: 'c09', image: '/static/img/c09.png', flipped: false, found: false }
      ]
    }
  },
  methods: {
    startGame () {
      this.shuffleCards()
    },
    flipCard (card) {
      card.flipped = true
      this.scores++
      return true
    },
    shuffleCards () {
      console.log('shuffling... ')
      let j, temp
      for (let i = this.cards.length - 1; i > 0; i--) {
        j = Math.floor(Math.random() * (i + 1))
        temp = this.cards[j]
        this.cards[j] = this.cards[i]
        this.cards[i] = temp
      }
      console.log(this.cards)
      this.$forceUpdate()
    }
  },
  mounted () {
    this.$root.$on('reset-game', () => {
      console.log('reset-game catched')
      this.startGame()
    })
  }
}
</script>

<style scoped>
  .cards {
    max-width: 872px;
    margin: 0 auto;
  }
  .card {
    position: relative;
    display: inline-block;
    width: 92px;
    height: 137px;
    margin: 10px 20px;
  }

  .front, .back {
    position: absolute;
    left: 0; right: 0; top: 0; bottom: 0;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    transform: translateZ(0);
    transition: transform 0.6s;
    transform-style: preserve-3d;
  }

  .back {
    background-image: url('/static/img/back.png');
    background-size: 100%;
    background-position: center;
    background-repeat: no-repeat;
    font-size: 12px;
  }

  .front {
    transform: rotateY(-180deg);

    background-size: 100%;
    background-repeat: no-repeat;
    background-position: center;
  }

  .flipped .back, .found .back {
    transform: rotateY(180deg);
  }

  .flipped .front, .found .front {
    transform: rotateY(0deg);
  }
</style>
