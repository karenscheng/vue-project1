<template>
  <div class="hello">
    <div class="hero">
      <h1 contenteditable>WHICH GALAXY ARE YOU?</h1>
    </div>
    <button @click="loadQuestion"><span>TAKE THE QUIZ</span></button>
  </div>
</template>

<script>

export default {

  mounted () {
    const hero = document.querySelector('.hero')
    const text = hero.querySelector('h1')
    const walk = 30 // 100px

    function shadow (e) {
      const width = hero.offsetWidth
      // const height = hero.offsetHeight
      let { offsetX: x, offsetY: y } = e

      if (this !== e.target) {
        x = x + e.target.offsetLeft
        y = y + e.target.offsetTop
      }

      const xwalk = (x / width * walk) - (walk / 2)
      const ywalk = (y / width * walk) - (walk / 2)

      text.style.textShadow = `
        ${xwalk}px ${ywalk}px 0 rgba(255,0,255,0.8)
      `
    }

    hero.addEventListener('mousemove', shadow)
  },

  methods: {
    loadQuestion () {
      this.$emit('buttonClicked')
    }
  }
}

</script>

<style scoped>

  .hello{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  h1{
    /*border: 5px solid red;*/
    background: url("/vue-project1/static/images/cluster.jpg") no-repeat;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    font-size: 16vmin;
    text-align: center;
    font-weight: bold;
    text-shadow: 5px 5px 0 rgba(255,0,255,0.8);
  }

  button{
    display: flex;
    background-color: rgba(15, 82, 186, .6);
    border: none;
    color: white;
    padding: 15px 32px;
    text-align: center;
    text-decoration: none;
    align-content: center;
    font-size: 4vmin;
    cursor: pointer;
    margin-top: -50px;
  }

  /*button span{
    color: rgba(255, 255, 255, 0.6);
  }*/

  @media only screen and (max-width: 1250px) {

    h1{
      font-size: 14vmin;
    }
  }

</style>
