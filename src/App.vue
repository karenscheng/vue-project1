<template>
  <div id="app">
    <div id="panel">
        <Hello @buttonClicked="handleClick" v-show="!questionTime"></Hello>
      <!-- <transition-group name="slide"> -->
        <Question @backClicked="goBack" @answerStored="nextQuestion" v-show="questionTime" v-if="currentQuestion === question" v-for="(question, index) in questions" :question="question" :index="index" v-bind:key="index"></Question>
      <!-- <transition-group> -->
        <Result v-show="showResult" :picture="this.picture" :address="this.address" :description="this.description"></Result>
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
      questionTime: false, // activates after homepage
      questions: [],
      currentQuestion: null,
      questionIndex: 0,
      showResult: false,
      responses: [],
      resultKeys: [],
      picture: null,
      address: null,
      description: null
    }
  },

  mounted () {
    console.log('App -> mounted.')
    axios.get('../../static/quiz.json')
      .then((response) => {
        this.questions = response.data
      })
  },

  components: {
    Hello,
    Question,
    Result
  },

  methods: {
    handleClick () {  // triggers after "Take the quiz" button is clicked
      console.log('App -> button clicked.')
      this.questionTime = true
      this.currentQuestion = this.questions[this.questionIndex]
    },

    goBack () {   // go back to previous question
      this.questionIndex--
      console.log('App -> questionIndex: ' + this.questionIndex)
      this.responses.splice(this.questionIndex, 1)  // removes previously recorded answers
      this.resultKeys.splice(this.questionIndex, 1)
      this.currentQuestion = this.questions[this.questionIndex]
    },

    nextQuestion (a) {  // triggers when an answer choice is clicked
      console.log('App -> nextQuestionClicked')
      this.responses.push(a)
      this.resultKeys.push(a.charAt(0))
      this.questionIndex++
      if (this.questionIndex === 9) { // shows Result component after question 9
        this.calculateResult()
        this.showResult = true
      }
      this.currentQuestion = this.questions[this.questionIndex]
    },

    calculateResult () {  // complicated algo for figuring out which galaxy you are
      console.log('App -> calculateResult: resultKeys = ' + this.resultKeys)
      let result = this.resultKeys.join('')

      // here I am checking to give certain answers a galaxy type
      if (this.resultKeys[1] === 'I' && this.resultKeys[2] === 'T' &&
        this.resultKeys[3] === 'T' && this.resultKeys[6] === 'T' &&
        this.resultKeys[8] === 'F') {
        result = 'mysterious'
      }
      if (this.resultKeys[0] === 'T' && this.resultKeys[1] === 'E' &&
        this.resultKeys[4] === 'T' && this.resultKeys[8] === 'M') {
        result = 'sombrero'
      } else if ((this.resultKeys[1] === 'E' && this.resultKeys[4] === 'S') ||
      (this.resultKeys[1] === 'E' && this.resultKeys[5] === 'S') ||
      (this.resultKeys[5] === 'S' && this.resultKeys[6] === 'S')) {
        result = 'normal'
      }
      console.log(this.result)

      /*
      1: T/F        Drama
      2: I/E/S      Introvert/Extrovert
      3: T/F        Old soul
      4: T/F        Distant
      5: T/S/F      Spontaneous
      6: T/S/F      Reflective
      7: T/F        Reserved
      8: T/F        Sympathetic
      9: F/M        Few friends/Many acquaintances
      */

      switch (result) { // sets image
        case 'sombrero':
          this.picture = 'Sombrero'
          break
        case 'normal':
          this.picture = 'NGC300'
          break
        case 'mysterious':
          this.picture = 'Circinus'
          break
        default:
          this.picture = 'Small Magellanic Cloud'
          break
      }
      this.setText(this.picture) // calls setText to set description
      this.address = '/static/images/' + this.picture + '.jpg'
      console.log('App -> ' + this.picture)
    },

    setText (pic) { // sets description
      switch (pic) {
        case 'NGC300':
          this.description = 'NGC300 is so interesting because it is so normal. It\'s a classic spiral galaxy, and is 7 million light years away. You are quite a normal person.'
          break
        case 'Andromeda': // add more personality stuff here
          this.description = 'Andromeda is the Milky Way\'s glactic neighbor. It is a spiral galaxy 2.5 million light years away, and it is thought that the Milky Way and Andromeda Galaxies will collide several billion years from now. The black holes located in both galaxies will then reside in the large, elliptical galaxy that results from this merger.'
          break
        case 'Sombrero':
          this.description = 'M104, also known as the Sombrero Galaxy, is unusually large and has an extended central bulge of stars. You are similar in that you have many friends and are usually the center of drama.'
          break
        case 'Circinus':
          this.description = 'The Circinus Galaxy is one of the nearest galaxies, but widely unexplored because the Milky Way veils it. Similar to you, its mysterious nature is quite curious.'
          break
        case 'Small Magellanic Cloud':
          this.description = 'welp. here is the default option.'
          break
      }
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
  position: absolute;
}
.slide-fade-enter, .slide-fade-leave-to {
  transform: translateX(10px);
  opacity: 0;
  position: absolute;
}

</style>
