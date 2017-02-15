<template>
  <div id="app">
    <div id="panel">
      <Hello></Hello>
    </div>
  </div>
</template>

<script>
import Hello from './components/Hello'

export default {
  name: 'app',
  components: {
    Hello
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: rgb(255, 255, 255);
  background-image: url("../images/cluster.jpg");
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

</style>
