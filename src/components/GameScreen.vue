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
    <button class="game-wrapper__button" @click="finishGame">Завершить игру</button>
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
      flippedCards: [null, null],
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
      // тут добавить старт таймера
    },

    flipCard (card) {
      if (card.flipped || !this.canFlipCard()) {
        return false
      }
      card.flipped = true
      if (this.flippedCards[0] !== null) {
        this.flippedCards[1] = card
        this.checkTurn()
      } else {
        this.flippedCards[0] = card
      }
      return true
    },

    checkTurn () {
      if (this.flippedCards[0].name === this.flippedCards[1].name) {
        this.calcScore(true)
        window.setTimeout(() => {
          this.flippedCards[0].found = true
          this.flippedCards[1].found = true
          this.clearFlippedCards()
        }, 500)
      } else {
        this.calcScore(false)
        window.setTimeout(() => {
          this.flippedCards[0].flipped = false
          this.flippedCards[1].flipped = false
          this.clearFlippedCards()
        }, 1000)
      }
    },

    calcScore (isFound) {
      let turnScore = 0
      let finished = false
      if (isFound) {
        for (let i = 0; i < this.cards.length; i++) {
          if (!this.cards[i].flipped && !this.cards[i].found) {
            turnScore++
          }
        }
        finished = (turnScore === 0)
        turnScore = turnScore / 2
      } else {
        for (let i = 0; i < this.cards.length; i++) {
          if (this.cards[i].found) {
            turnScore++
          }
        }
        turnScore = -1 * turnScore / 2
      }
      if (this.scores + turnScore < 0) {
        this.scores = 0
      } else {
        this.scores += turnScore
      }
      if (finished) {
        window.setTimeout(() => {
          this.finishGame()
        }, 600)
      }
    },

    clearFlippedCards () {
      this.flippedCards[0] = null
      this.flippedCards[1] = null
    },

    canFlipCard () {
      return this.flippedCards[0] === null || this.flippedCards[1] === null
    },

    shuffleCards () {
      this.closeAll()
      let j, temp
      for (let i = this.cards.length - 1; i > 0; i--) {
        j = Math.floor(Math.random() * (i + 1))
        temp = this.cards[j]
        this.cards[j] = this.cards[i]
        this.cards[i] = temp
      }
      this.$forceUpdate()
    },

    showAll () {
      for (let i = 0; i < this.cards.length; i++) {
        this.cards[i].flipped = true
      }
    },

    closeAll () {
      for (let i = 0; i < this.cards.length; i++) {
        if (!this.cards[i].found) {
          this.cards[i].flipped = false
        }
      }
    },

    finishGame () {
      this.$root.$emit('finish-game', this.scores)
    },

    /* Расширение игры: таймер, ограничивающий время открытия всех карточек
    *  Настроить тут таймеры, обновляющие отображаемое оставшееся время раз в секунду и
    *  завершающие игру по истечении времени
    */
    startTimer () {
      return true
    }
  },

  mounted () {
    this.startGame()
    window.setTimeout(() => { this.showAll() }, 300)
    window.setTimeout(() => { this.closeAll() }, 5300)
  }
}
</script>

<style scoped>
  .cards {
    max-width: 872px;
    margin: 0 auto 4em;
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

  .found {
    visibility: hidden;
  }

  .game-wrapper__button {
    display: inline-block;
    border-radius: 1.1em;
    background-color: deepskyblue;
    font-size: x-large;
    color: white;
    padding: .5em 3em;
    border: none;
    cursor: pointer;
  }
</style>
