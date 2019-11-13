<template>
  <div class="end-wrapper">
    <img class="end-wrapper__img" src="/static/img/finish.jpg" alt="">
    <h1>Игра завершена!</h1>
    <h2>Вы набрали {{ scores }}</h2>
    <button class="end-wrapper__button" @click="startGame">
      Повторить игру
    </button>
  </div>
</template>

<script>
export default {
  name: 'EndScreen',

  data () {
    return {
      scores: ''
    }
  },

  methods: {
    startGame () {
      this.$root.$emit('start-game')
    }
  },

  mounted () {
    this.$root.$on('game-scores', (scores) => {
      let scoresStr = scores.toString()
      let digit = parseInt(scoresStr.substr(scoresStr.length - 1))
      let str = 'балл'
      if ((scores > 4 && scores < 21) || digit === 0 || (digit >= 5 && digit <= 9)) {
        str += 'ов'
      } else if (digit >= 2 && digit <= 4) {
        str += 'а'
      }
      this.scores = scores + ' ' + str
    })
  }
}
</script>

<style scoped>
  .end-wrapper {
    margin-top: 15%;
    color: white;
  }

  .end-wrapper__img {
    width: 200px;
    border-radius: 50%;
  }

  .end-wrapper__button {
    display: inline-block;
    border-radius: 1.1em;
    background-color: deepskyblue;
    font-size: x-large;
    color: white;
    padding: .5em 3em;
    border: none;
  }

  h1 {
    margin: 1em 0 .5em;
  }

  h2 {
    margin: .5em 0 2em;
    font-weight: normal;
  }
</style>
