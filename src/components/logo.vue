<template>
  <div class="logoMain">
    <img :src="logoSrc[logoSelected]">
    <span class="logoTriangle" v-on:click="showFlag=!showFlag"></span>
    <transition name="logoShow">
      <ul v-show="showFlag" class="logoList">
        <li v-for="(item,key) in logoSrc" v-on:click="logoChoice(key)">
          <img :src="item"/>
        </li>
      </ul>
    </transition>
  </div>
</template>
<script>
  export default {
    data: function () {
      return {
        logoSelected: 0,
        showFlag:false,
        logoSrc: [
          require('../assets/baidu_logo.png')
          ,
          require('../assets/360_logo.png')
          ,
          require('../assets/sougou_logo.png')
        ]
      }
    },
    methods:{
        logoChoice:function (key) {
            this.$emit("choice",key);
            console.log("key: ",key);
            this.logoSelected=key;
            this.showFlag=!this.showFlag;
        }
    }
  }
</script>
<style type="text/css">
  .logoMain {
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
  }

  .logoTriangle {
    height: 0;
    width: 0;
    border: 10px solid;
    border-color: #000 transparent transparent transparent;
    transform: translate(-4rem, 0.4rem);
    cursor: pointer;
  }

  .logoList{
    display: block;
    box-sizing: border-box;
    margin: 0;
    padding: 0;
    position: absolute;
    list-style: none;
    width: 230px;
    top: 100%;

    z-index: 999;
    border: 1px solid lightsteelblue;
  }

  .logoList li{
    width: 100%;
    height: 5rem;
    line-height: 5rem;
    display: flex;
    align-items: center;
  }

  .logoList li:hover{
    background-color: wheat;
  }

  .logoList li img{
    display: block;
    width: 100%;
    cursor: pointer;
  }

  .logoShow-enter-active,.logoShow-leave-active{
    transition: all .5s;
  }
  .logoShow-enter,.logoShow-leave-active{
    opacity: 0;
    transform: translateY(-20px);
  }
</style>
