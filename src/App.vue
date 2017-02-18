<template>
  <div id="app">
    <div id="panel">
      <!-- #failing at getting these dumb transitions to work  -->
      <!-- <transition name="slide"> -->
        <Hello @buttonClicked="handleClick" v-show="!questionTime"></Hello>
      <!-- </transition> -->
      <!-- <transition-group name="slide"> -->
        <Question @backClicked="goBack" @answerStored="nextQuestion" v-show="questionTime" v-if="currentQuestion === question" v-for="(question, index) in questions" v-bind:key="index" :question="question" :index="index"></Question>
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
      this.resultKeys.push(a.charAt(0)) // resultKeys stores the first letter of each answer choice
      this.questionIndex++
      if (this.questionIndex === 9) { // shows Result component after question 9
        this.calculateResult()
        this.showResult = true
      }
      this.currentQuestion = this.questions[this.questionIndex]
    },

    calculateResult () {  // complicated algo for figuring out which galaxy you are
      console.log('App -> calculateResult: resultKeys = ' + this.resultKeys)
      let result = this.resultKeys.join('') // makes resultKeys into a single string

      // here I am checking to give certain answers a galaxy type
      if (this.resultKeys[1] === 'I' && this.resultKeys[2] === 'T' &&
        this.resultKeys[3] === 'T' && this.resultKeys[6] === 'T' &&
        this.resultKeys[8] === 'F') {
        result = 'mysterious'
      } else if (this.resultKeys[0] === 'F' && this.resultKeys[1] === 'I' &&
        this.resultKeys[2] === 'T' && this.resultKeys[4] === 'T' &&
        this.resultKeys[5] === 'T') {
        result = 'backwards-spiral'
      } else if (this.resultKeys[2] === 'T' && this.resultKeys[3] === 'T' &&
        this.resultKeys[5] === 'T' && this.resultKeys[6] === 'T' &&
        this.resultKeys[7] === 'T') {
        result = 'wise'
      } else if (this.resultKeys[0] === 'T' && this.resultKeys[1] === 'E' &&
        this.resultKeys[3] === 'F' && this.resultKeys[4] === 'T' &&
        this.resultKeys[8] === 'F') {
        result = 'centarusA'
      } else if (this.resultKeys[0] === 'T' && this.resultKeys[1] === 'E' &&
        this.resultKeys[4] === 'T' && this.resultKeys[8] === 'M') {
        result = 'sombrero'
      } else if (this.resultKeys[0] === 'T' && this.resultKeys[1] === 'E' &&
        this.resultKeys[5] === 'T' && this.resultKeys[6] === 'F' &&
        this.resultKeys[8] === 'M') {
        result = 'starburst'
      } else if (this.resultKeys[1] === 'I' && this.resultKeys[3] === 'T' &&
        this.resultKeys[4] === 'T') {
        result = 'ngc3079'
      } else if (this.resultKeys[0] === 'T' && this.resultKeys[4] === 'T' &&
        this.resultKeys[8] === 'F') {
        result = 'ngc1097'
      } else if (this.resultKeys[0] === 'T' && this.resultKeys[1] === 'E') {
        result = 'm100'
      } else if (this.resultKeys[0] === 'T' && this.resultKeys[7] === 'F') {
        result = 'pinwheel'
      } else if ((this.resultKeys[1] === 'S' && this.resultKeys[4] === 'S') ||
      (this.resultKeys[1] === 'S' && this.resultKeys[5] === 'S') ||
      (this.resultKeys[4] === 'S' && this.resultKeys[5] === 'S')) {
        result = 'normal'
      }
      console.log(this.result)

      /* answers    Questions                         resultKeys index
      0: T/F        Drama                             resultKeys[0]
      1: I/E/S      Introvert/Extrovert               resultKeys[1]
      2: T/F        Old soul                          resultKeys[2]
      3: T/F        Distant                           resultKeys[3]
      4: T/S/F      Spontaneous                       resultKeys[4]
      5: T/S/F      Reflective                        resultKeys[5]
      6: T/F        Reserved                          resultKeys[6]
      7: T/F        Sympathetic                       resultKeys[7]
      8: F/M        Few friends/Many acquaintances    resultKeys[8]
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
        case 'backwards-spiral':
          this.picture = 'Backwards Spiral'
          break
        case 'wise':
          this.picture = 'M81'
          break
        case 'centarusA':
          this.picture = 'Centarus A'
          break
        case 'starburst':
          this.picture = 'Starburst'
          break
        case 'ngc3079':
          this.picture = 'NGC3079'
          break
        case 'ngc1097':
          this.picture = 'NGC1097'
          break
        case 'pinwheel':
          this.picture = 'Pinwheel'
          break
        case 'm100':
          this.picture = 'M100'
          break
        default:
          this.picture = 'Whirlpool'
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
        case 'Backwards Spiral':
          this.description = 'The Backwards Sprial Galaxy is quite puzzling to Astronomers, because it is a rare example of a sprial galaxy with arms pointing in opposite directions. Similar to you, it is quirky and hard to categorize.'
          break
        case 'Centarus A':
          this.description = 'The Centarus A is the 5th brightest galaxy in our sky, and is famous for its prominent dust lane across the middle with a giant jet blasting away from the supermassive black hole at the center. Like you, it garners a lot of attention, and people naturally gravitate towards you.'
          break
        case 'Starburst':
          this.description = 'The Starburst is 12 million light years away, and is the nearest place to us where the conditions are similar to those in the early universe, with lots of stars forming. It is vibrant and interesting, just like you!'
          break
        case 'M81':
          this.description = 'The M81 Galaxy is found in the northern constellation of Ursa Major, and is 12 million light years away. It hints at a disorderly past, with a remarkable dust lane running straight through the disk. It is similar to you in that you have had many experiences in the past that have shaped you into the wise person you are today.'
          break
        case 'NGC3079':
          this.description = 'The NGC3079 Galaxy is mysterious, and contains a bubble which was formed from winds of hot stars mixing with small bubbles of hot gas from supernova explosions. Spontaneous and full of surprises like you, but still keeps it distance  at 50 million light years away.'
          break
        case 'NGC1097':
          this.description = 'The NGC1097 Galaxy, like you, is incredibly interesting - in an 11 year span it had 3 supernovas (violent deaths of high-mass stars), and is 45 million light years away. You have a couple true friends that you always have by your side. NCG1097 also has two satellite galaxies that travel along with it #BFFS'
          break
        case 'Pinwheel':
          this.description = 'The M101 Galaxy, also known as the Pinwheel Galaxy, is 70% larger than our Milky Way and is 21 million light years away from Earth. It is so large that it distorts the smaller nearby galaxies, which is similar to you in that you sometimes have a hard time relating to others.'
          break
        case 'M100':
          this.description = 'The M100 Galaxy has conditions similar to the Milky Way, with over 100 billion stars. It is a large spiral galaxy, and astronomers have deemed it to be of grand design. It is majestic like you, but also can be extremely hot or cold depending on the spot.'
          break
        case 'Whirlpool':
          this.description = 'The M51 Galaxy, also known as the Whirlpool Galaxy, is 30 million light years away and is approximately 60,000 light years across. It\'s one of the brightest and most picturesque galaxies, and is in the process of merging with a smaller galaxy. It\'s kind of a wildcard, just like you.'
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
