<template>
  <div>
    <logo v-on:choice="getChoice"></logo>
    <div class="searchMain">
      <input type="text" v-model="getInput" v-on:input="changeInput"/><!--v-on:input="getInput"-->
      <span class="searchClear" v-show="showClear" v-on:click="inputClear">×</span>
      <button class="searchButton" v-bind:style="{backgroundColor:searchName[logoIndex].color}">
        {{searchName[logoIndex].name}}


      </button>
    </div>
  </div>
</template>
<script>
  import logo from './logo.vue';
  export default {
    components: {
      logo
    },
    data: function () {
      return {
        getInput: '',
        logoIndex: 0,
        searchName: [
          {name: '百度一下', color: '#38f'},
          {name: '搜一下', color: '#19cd55'},
          {name: '搜狗', color: '#ff5943'}
        ],
        buttonColor: '',
        showClear: false,
      }
    },
    methods: {
      getChoice: function (key) {
        this.logoIndex = key;
        let inp = document.getElementsByTagName("INPUT")[0];
        inp.onfocus = ()=> {
          inp.style.borderColor = this.searchName[this.logoIndex].color;
        };
        inp.onblur = function () {
          inp.style.borderColor = '';
        }
      },
      /*  getInput: function (inp) {
       if((inp.target.value)!=""){
       this.showClear=true;
       }else
       this.showClear=false;
       },*/
      changeInput: function () {
        if ((this.getInput) !== "") {
          this.showClear = true;
        } else
          this.showClear = false;
      },
      inputClear: function () {
        this.getInput = '';
        this.showClear = false;
      }
    }

  }
  /**/
</script>
<style type="text/css">
  * {
    margin: 0;
    padding: 0;
  }

  .searchMain input {
    font-size: 1.4rem;
    width: 38.5rem;
    height: 2.8rem;
    position: relative;
    box-sizing: border-box;
  }

  .searchMain input:focus {
    box-sizing: border-box;
    border: 1px solid #38f;
    outline: none;
    padding-left: 0.6rem;
  }

  .searchMain span {
    font-size: 1.5rem;
    position: absolute;
    transform: translate(-1.8rem, .6rem);
    cursor: default;
  }

  .searchMain span:hover {
    transform: translate(-1.8rem, .6rem) scale(1.3);
  }

  .searchButton {
    position: absolute;
    display: inline-block;
    color: white;
    font-size: 1.1rem;
    width: 7.2rem;
    height: 2.8rem;
    line-height: 2.7rem;
    font-family: arial;
    cursor: pointer;
    border: 0;
    padding: 0;
    font-weight: normal;
  }
</style>
