<template>
  <div>
    <logo v-on:choice="getChoice"></logo>
    <div class="searchMain">
      <input type="text" v-model="getInput" v-on:input="changeInput" @keyup.enter="buttonClick" @keyup.down="pressDown"
             @keyup.up="pressUp()"/>
      <!--v-on:input="getInput"-->
      <span class="searchClear" v-show="showClear" v-on:click="inputClear">×</span>
      <button class="searchButton" v-bind:style="{backgroundColor:searchName[logoIndex].color}"
              v-on:click="buttonClick">
        {{searchName[logoIndex].name}}
      </button>
      <div class="searchList">
        <transition name="suggestShow">
          <ul v-show="showList">
            <li v-for="(item,key) in searchSuggest" v-on:click="listClick(item)"
                v-bind:class="{searchListHover:key==choiceIndex}" @mouseover="mouseHover(key)">
              {{item}}
            </li>
          </ul>
        </transition>
      </div>
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
        choiceIndex: -1,
        showList: false,
        searchName: [
          {
            name: '百度一下', color: '#38f',
            searchSrc: 'https://www.baidu.com/s?ie=utf-8&f=8&rsv_bp=0&rsv_idx=1&tn=baidu&wd=',
            suggestSrc: 'http://suggestion.baidu.com/su?wd=',
            searchjsonp: 'cb'
          },
          {
            name: '搜一下', color: '#19cd55',
            searchSrc: 'https://www.so.com/s?ie=utf-8&shb=1&src=360sou_newhome&q=',
            suggestSrc: 'https://sug.so.360.cn/suggest?word=',
            searchjsonp: ''
          },
          {
            name: '搜狗', color: '#ff5943',
            searchSrc: 'https://www.sogou.com/web?query=',
            //suggestSrc: 'https://www.sogou.com/suggnew/ajajjson?type=web&key='
            suggestSrc:'http://suggestion.baidu.com/su?wd=',
            searchjsonp:'cb'
          }
        ],
        buttonColor: '',
        showClear: false,
        searchSuggest: []
      }
    },
    methods: {
      getChoice: function (key) {
        this.logoIndex = key;
        let inp = document.getElementsByTagName("INPUT")[0];
        inp.onfocus = () => {
          inp.style.borderColor = this.searchName[this.logoIndex].color;
        };
        inp.onblur = function () {
          inp.style.borderColor = '';
        }
      },
      getInput: function (inp) {
        if ((inp.target.value) !== "") {
          this.showClear = true;
        } else
          this.showClear = false;
      },
      changeInput: function () {
        if ((this.getInput) !== "") {
          this.showClear = true;
          this.$http.jsonp(this.searchName[this.logoIndex].suggestSrc + this.getInput, {
            jsonp: this.searchName[this.logoIndex].searchjsonp
          })
            .then(function (response) {
              this.searchSuggest = response.data.s;
              if (this.searchSuggest.length !== 0)
                this.showList = true;
              else
                this.showList = false;
            }).catch(e => {
            console.log(e)
          });
        } else {
          this.showClear = false;
          this.showList = false;
        }
      },
      inputClear: function () {
        let inp = document.getElementsByTagName("INPUT")[0];
        this.getInput = '';
        this.showClear = false;
        inp.focus();
      },
      buttonClick: function () {
        window.location.href = this.searchName[this.logoIndex].searchSrc + this.getInput;
      },
      listClick: function (item) {
        window.location.href = this.searchName[this.logoIndex].searchSrc + item;
      },
      pressUp: function (item) {
        if (this.choiceIndex === -1)
          this.choiceIndex = this.searchSuggest.length - 1;
        else if (--this.choiceIndex === -1)
          this.choiceIndex = this.searchSuggest.length - 1;
        this.getInput = this.searchSuggest[this.choiceIndex];
      },

      pressDown: function () {
        this.choiceIndex++;
        if (this.choiceIndex === this.searchSuggest.length)
          this.choiceIndex = 0;
        this.getInput = this.searchSuggest[this.choiceIndex];
      },
      mouseHover: function (key) {
        this.choiceIndex = key;
      }
    },
    mounted(){
      let inp = document.getElementsByTagName("INPUT")[0];

      function stopPropagation(event) {
        if (event.stopPropagation) {
          event.stopPropagation();
        } else {
          event.cancelBubble = true;
        }
      }

      window.addEventListener('click', () => {
        this.showList = false;
      })
      inp.addEventListener('click', stopPropagation);
    }
  }

</script>
<style type="text/css">
  * {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  .searchMain {
    position: relative;
    width: 100%;
    margin: 0 auto;
  }

  .searchMain input {
    font-size: 1.4rem;
    width: 38.5rem;
    height: 2.8rem;
    position: relative;
    box-sizing: border-box;
    padding-left: 0.6rem;

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

  .searchList {
    width: 100%;
    height: 2rem;
    background-color: white;
    z-index: 999;
    margin: 0;
    padding: 0;

  }

  .searchList ul {
    margin: 0 auto;
    width: 38.5rem;
    border-width: 0 1px 1px 1px;
    border-style: solid;
    border-color: #ccc;
    box-shadow: 1px 1px 3px #ededed;
    box-sizing: border-box;
  }

  .searchList li {
    width: 100%;
    font-size: 1rem;
    line-height: 1.8rem;
    cursor: default;
    text-align: left;
    padding: 0 8px;
    box-sizing: border-box;
  }

  .searchListHover {
    transition: all 0.6s;
    background-color: #F0F0F0;
  }

  .suggestShow-enter-active, .suggestShow-leave-active {
    transition: all .5s;
  }

  .suggestShow-enter, .suggestShow-leave-active {
    opacity: 0;
    transform: translateY(-20px);
  }
</style>
