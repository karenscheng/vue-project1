<template>
  <div class="Question">
    <button @click="back">BACK</button>
    <h1>Question # {{ currentQuestion }}</h1>
    <h2 class="q">{{ question.question }}</h2>
    <ul class="choices">
      <li>{{ question.answers }}</li>
    </ul>
  </div>
</template>

<script>

export default {
  props: [
    'question'
  ],

  data () {
    return {
      currentQuestion: 1
    }
  },

  methods: {
    back () {
      this.$emit('backClicked')
    }
  }
}
</script>

<style>

h1, h2, ul{
  color: black
}

</style>
