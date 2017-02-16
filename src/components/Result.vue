<template>
  <div class="result">
    <h1>You are the {{ }} ______ Galaxy!</h1>
    <img src='../../static/images/M2.jpg'></img>
    <h2>This is a super great galaxy and you should be proud. {{ }}<h2>
  </div>
</template>

<script>

export default {

}

</script>

<style>

.result{
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

img{
  width: auto;
  height: 50vh;
}

</style>
