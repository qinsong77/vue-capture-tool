<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link>
      |
      <router-link to="/about">About</router-link>
    </div>
    <router-view/>
   <transition name="fade">
     <div class="canvas-container" v-show="showContainer">
       <button class="close-container" @click="closeContainer">X</button>
     </div>
   </transition>
    <div class="operation-container">
      <button class="capture" @click="capture">Capture</button>
    </div>
  </div>
</template>
<script>
  import html2canvas from 'html2canvas'

  export default {
    data(){
      return {
        showContainer: false,
      }
    },
    methods: {
      capture() {
        this.showContainer = true
        html2canvas(document.body).then(canvas => {
          document.querySelector('.canvas-container').appendChild(canvas)
        }).catch(error=>{
          console.log(error)
        })
      },
      closeContainer(){
        this.showContainer = false
        setTimeout(()=>{
          document.querySelector('.canvas-container').removeChild(document.querySelector('canvas'))
        },270)
      }
    }
  }
</script>
<style lang="scss">
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
  }
  .canvas-container{
    position: fixed;
    top:0;
    right: 0;
    left: 0;
    bottom:0;
    width: 100%;
    height: 100%;
    z-index: 999;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
    >canvas{
      width: 80%!important;
      height: 80%!important;
    }
    .close-container{
      position: absolute;
      top:60px;
      right: 50px;
      padding: 5px 13px;
      background-color: transparent;
      outline: none;
      border: 1px solid beige;
    }
  }
  .operation-container {
    position: fixed;
    right: 90px;
    bottom: 100px;
    > .capture {
      box-sizing: border-box;
      background: transparent;
      padding: 5px 10px;
      border-radius: 5px;
      border: 1px solid #a7ff81;
      outline: none;
      cursor: pointer;
      transition: background-color ease-in-out .3s;
      &:hover {
        background-color: #1ed8e6;;
      }
    }
  }
  .fade-enter-active,.fade-leave-active {
    transition: all .3s ease-in-out;
  }
  .fade-enter-to,.fade-leave,.fade-leave-to{
    opacity: 1;
    transform: scale(1,1);
  }
  .fade-enter,.fade-leave-to{
    opacity: 0;
    transform: scale(0.3,0.3);
  }
  #nav {
    padding: 30px;
    a {
      font-weight: bold;
      color: #2c3e50;
      &.router-link-exact-active {
        color: #42b983;
      }
    }
  }
</style>
