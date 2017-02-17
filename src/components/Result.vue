<template>
  <div class="result">
    <h1>You are the <span class="blue"> {{ this.picture }} Galaxy</span>!</h1>
    <img :src="address"></img>
    <p>{{ this.description }}<p>
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
      spaceImage: '/static/images/' + this.picture + '.jpg'
    }
  }
}

</script>

<style>

.result {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

img {
  width: auto;
  height: 50vh;
}

p {
  color: black;
  font-size: 16px; /* make this em later, also test for responsiveness */
  font-weight: bold;
  text-align: center;
}

.blue {
  color: rgb(15, 82, 186);
}

</style>
