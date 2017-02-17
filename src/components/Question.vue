<template>
  <!-- <transition name="slide"> -->
    <div class="Question">
      <h1>Question # {{ currentQuestion }}</h1>
      <h2 class="q">{{ question.question }}</h2>
      <ul class="choices">
        <li @click="storeChoice(answer)" v-for="answer in question.answers">{{ answer }}</li>
      </ul>
      <button v-show="index!==0" @click="back" id="backButton">Previous Question</button>
      <button v-show="index===0" id="fakeBackButton">Previous Question</button>
    </div>
  <!-- </transition> -->
</template>

<script>

export default {
  props: [
    'question',
    'index'
  ],

  mounted () {
    console.log('Question -> ' + this.index)
  },

  data () {
    return {
      currentQuestion: this.index + 1
    }
  },

  methods: {
    back () {
      this.$emit('backClicked')
    },

    storeChoice (a) {
      this.$emit('answerStored', a)
    }
  }
}
</script>

<style>

.Question {
  /*border: 5px solid red;*/
  width: 60vw;
  height: 70vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

h1, h2, ul {
  color: black
}

h1 {
  font-size: 6vmin
}

h2 {
  font-size: 4vmin
}

.choices {
  /*border: 5px solid red;*/
  display: flex;
  justify-content: space-between;
  /*flex-wrap: wrap;*/
}

.choices li{
  /*border: 2px solid blue;*/
  list-style: none;
  text-align: center;
  background-color: rgba(15, 82, 186, .6);
  color: white;
  font-size: 20px;
  font-weight: bold;
  margin: 10px;
  text-decoration: none;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100px;
  padding: 10px;
  flex: 1;
}

#backButton {
  width: 15vw;
  margin-top: 20px;
  height: 10vh;
  padding: 20px;
  background-color: rgba(35, 185, 160, .9);
  border: none;
  color: white;
  font-weight: bold;
  text-align: center;
  text-decoration: none;
  align-content: center;
  font-size: 3vmin;
  cursor: pointer;
  padding: 5px;
}

#fakeBackButton {
  width: 15vw;
  height: 10vh;
  margin-top: 20px;
  background-color: rgba(35, 185, 160, .4);
  border: none;
  color: white;
  font-weight: bold;
  text-align: center;
  text-decoration: none;
  align-content: center;
  font-size: 3vmin;
  padding: 5px;
}

/*@media only screen and (max-width: 400px) {
  h1{
    font-size: 10vh;
  }
}*/

/*.slide-fade-enter-active {
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
}*/

</style>
