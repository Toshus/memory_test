<template>
  <div class="content">
    <component :is="currentView"></component>
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
  },

  methods: {
    startGame () {
      this.currentView = GameScreen
    }
  }
}
</script>

<style scoped>
.content {
  padding: 1px;
}
</style>
