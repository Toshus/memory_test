<template>
  <div class="content">
    <component :is="currentView" ref="comp"></component>
  </div>
</template>

<script>
import StartScreen from './StartScreen'
import GameScreen from './GameScreen'
import EndScreen from './EndScreen'

export default {
  name: 'MainContainer',

  components: {
    'start-screen': StartScreen,
    'game-screen': GameScreen,
    'end-screen': EndScreen
  },

  data () {
    return {
      currentView: StartScreen
    }
  },

  mounted () {
    this.$root.$on('start-game', () => {
      this.startGame()
    })
    this.$root.$on('finish-game', (scores) => {
      this.finishGame(scores)
    })
  },

  methods: {
    startGame () {
      this.currentView = GameScreen
    },
    finishGame (scores) {
      this.currentView = EndScreen
      // небольшой хак, чтобы успел установиться обработчик события
      window.setTimeout(() => { this.$root.$emit('game-scores', scores) }, 0)
    }
  }
}
</script>

<style scoped>
.content {
  padding: 1px;
}
</style>
