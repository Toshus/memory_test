<template>
  <div class="start-wrapper">
    <img src="/static/img/c08.png" alt="">
    <h1>Игра "Memory"</h1>
    <button class="start-wrapper__button" @click="startGame">
      Начать игру!
    </button>
  </div>
</template>

<script>

export default {
  name: 'StartScreen',
  methods: {
    startGame () {
      this.$root.$emit('start-game')
    }
  }
}
</script>

<style scoped>
  .start-wrapper {
    margin-top: 20%;
    color: white;
  }
  .start-wrapper__button {
    display: inline-block;
    border-radius: 1.1em;
    background-color: deepskyblue;
    font-size: x-large;
    color: white;
    padding: .5em 3em;
    border: none;
  }

  h1 {
    margin: 2em 0 3em;
  }
</style>
