<template>
  <div class="result">
    <h1>You are the <span class="blue"> {{ this.picture }} Galaxy</span>!</h1>
    <img :src="address"></img>
    <p class="description">{{ this.description }}<p>
    <p id="credit">Image source: <a href="http://nasa.gov" target="_newtab">NASA</a></p>
  </div>
</template>

<script>

export default {
  props: [
    'picture',
    'address',
    'description'
  ],

  data () {
    return {
      spaceImage: '/vue-project1/static/images/' + this.picture + '.jpg'
    }
  }
}

</script>

<style>

.result {
  /*padding-top: 20px;*/
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  /*border: 1px solid red;*/
}

h1 {
  font-size: 5vmin;
  text-align: center;
}

img {
  width: 40vmin;
  height: auto;
}

.description {
  color: black;
  font-size: 3vmin; /* make this em later, also test for responsiveness */
  font-weight: bold;
  text-align: center;
}

.blue {
  color: rgb(15, 82, 186);
}

#credit {
  align-self: flex-end;
  font-size: 3vmin;
  font-weight: normal;
  font-style: italic;
  color: black;
}

@media only screen and (min-device-width : 375px)
    and (max-device-width : 667px)
    and (width : 375px)
    and (height : 559px)
    and (orientation : portrait)
    and (color : 8)
    and (device-aspect-ratio : 375/667)
    and (aspect-ratio : 375/559)
    and (device-pixel-ratio : 2)
    and (-webkit-min-device-pixel-ratio : 2) {

  .result {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }

  h1 {
    font-size: 5vmin;
  }

  img {
    height: 50vmin;
    width: auto;
  }

  .description {
    font-size: 2vh;
    color: black;
    font-weight: bold;
    text-align: center;
  }

  #credit {
    align-self: flex-end;
    font-size: 2vh;
    font-weight: normal;
    font-style: italic;
    color: black;
  }
}


</style>
