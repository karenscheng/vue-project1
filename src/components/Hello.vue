<template>
  <div class="hello">
    <h1>WHICH GALAXY ARE YOU?</h1>
    <button type="button"><span>TAKE THE QUIZ</span></button>
  </div>
</template>

<style scoped>

  .hello{
    display: flex;
    flex-direction: column; 
    justify-content: center;
    align-items: center;
  }

  h1{
    /*border: 5px solid red;*/
    background: url("../../images/cluster.jpg") no-repeat;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    font-size: 18vmin;
    text-align: center;
    font-weight: bold;
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
