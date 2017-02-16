<template>
  <div id="app">
    <div id="panel">
        <Hello @buttonClicked="handleClick" v-show="!questionTime"></Hello>
      <transition name="slide">
        <Question @backClicked="goBack" v-show="questionTime" v-for="question in questions" :question="question"></Question>
      <transition>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Hello from './components/Hello'
import Question from './components/Question'

export default {
  name: 'app',

  data () {
    return {
      questionTime: false,
      questions: []
    }
  },

  mounted () {
    console.log('App -> mounted.')
    axios.get('../../static/quiz.json')
      .then((response) => {
        console.log(response.data) // ex.: { user: 'Your User'}
        this.questions = response.data
      })
  },

  components: {
    Hello,
    Question
  },

  methods: {
    handleClick () {
      console.log('App -> button clicked.')
      this.questionTime = true
    },

    goBack () {
      this.questionTime = false
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: rgb(255, 255, 255);
  background-image: url("../static/images/cluster.jpg");
  display: flex;
  /*border: 5px solid red;*/
  justify-content: center;
  flex-direction: column;
  align-items: center;
  height: 100vh;
  overflow: hidden;
}

#panel{
  background: rgba(255, 255, 255, 0.6);
  height: 80vh;
  width: 70vw;
  display: flex;
  flex-direction: column;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  padding: 20px;
  /*border: 5px solid red;*/
}

.slide-fade-enter-active {
  transition: all .3s ease;
}
.slide-fade-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active for <2.1.8 */ {
  transform: translateX(10px);
  opacity: 0;
}

</style>
