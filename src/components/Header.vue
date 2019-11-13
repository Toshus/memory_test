<template>
  <div class="header">
    <div class="scores">
      <span>Баллов: {{ scores }}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Header',
  props: {
    scores: Number
  }
}
</script>

<style scoped>
  .header {
    margin: 2em 2em 4em;
    padding-bottom: 2em;
    text-align: right;
    border-bottom: white 1px solid;
  }
  .scores span {
    color: white;
    font-size: x-large;
  }
</style>
