<template>
  <div id="app">
    <div id="panel">
        <Hello @buttonClicked="handleClick" v-show="!questionTime"></Hello>
      <!-- <transition-group name="slide"> -->
        <Question @backClicked="goBack" @answerStored="nextQuestion" v-show="questionTime" v-if="currentQuestion === question" v-for="(question, index) in questions" :question="question" :index="index"></Question>
      <!-- <transition-group> -->
        <Result v-show="showResult"></Result>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Hello from './components/Hello'
import Question from './components/Question'
import Result from './components/Result'

export default {
  name: 'app',

  data () {
    return {
      questionTime: false,
      questions: [],
      currentQuestion: null,
      questionIndex: 0,
      showResult: false,
      responses: [],
      resultKeys: []
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
    Question,
    Result
  },

  methods: {
    handleClick () {
      console.log('App -> button clicked.')
      this.questionTime = true
      this.currentQuestion = this.questions[this.questionIndex]
    },

    goBack () {
      this.questionIndex--
      console.log('App -> questionIndex: ' + this.questionIndex)
      this.responses.splice(this.questionIndex, 1)
      this.resultKeys.splice(this.questionIndex, 1)
      this.currentQuestion = this.questions[this.questionIndex]
    },

    nextQuestion (a) {
      console.log('App -> nextQuestionClicked')
      this.responses.push(a)
      this.resultKeys.push(a.charAt(0))
      this.questionIndex++
      if (this.questionIndex === 9) {
        this.showResult = true
        this.calculateResult()
      }
      this.currentQuestion = this.questions[this.questionIndex]
    },

    calculateResult () {
      console.log('App -> calculateResult: resultKeys = ' + this.resultKeys)
      const result = this.resultKeys.join('')
      console.log(result)
      // const q1 = this.responses[0]
      // const q2 = this.responses[1]
      // const q3 = this.responses[2]
      // const q4 = this.responses[3]
      // const q5 = this.responses[4]
      // const q6 = this.responses[5]
      // const q7 = this.responses[6]
      // const q8 = this.responses[7]
      // const q9 = this.responses[8]
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
