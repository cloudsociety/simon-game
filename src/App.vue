<template>
  <div id="app">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 350 350">
      <!-- <title>
        Simon
      </title> -->

      <defs>
        <filter id="dropshadow" height="130%">
          <feGaussianBlur in="SourceGraphic" stdDeviation="6"/> <!-- stdDeviation is how much to blur -->
          <feComponentTransfer>
            <feFuncA type="linear" slope="0.5"/>
          </feComponentTransfer>
          <feMerge>
            <feMergeNode/> <!-- this contains the offset blurred image -->
            <feMergeNode in="SourceGraphic"/> <!-- this contains the element that the filter is applied to -->
          </feMerge>
        </filter>
      </defs>

      <circle class="shadow" cx="183" cy="183" r="150"/>
      <circle class="base" cx="175" cy="175.027" r="150"/>

      <path @click="humanPlay(1)" class="btn btn--green btn--1" d="M40.205 167.527C43.96 98.94 98.913 43.987 167.5 40.232v75.26c-27.14 3.384-48.65 24.895-52.036 52.035h-75.26z"/>
      <path @click="humanPlay(2)" class="btn btn--red btn--2" d="M182.5 40.232c68.587 3.756 123.54 58.708 127.295 127.295h-75.26c-3.384-27.14-24.895-48.65-52.035-52.035v-75.26z"/>
      <path @click="humanPlay(3)" class="btn btn--blue btn--3" d="M309.795 182.527c-3.755 68.588-58.708 123.54-127.295 127.296v-75.26c27.14-3.384 48.65-24.895 52.036-52.036h75.26z"/>
      <path @click="humanPlay(4)" class="btn btn--yellow btn--4" d="M167.5 309.795C98.913 306.04 43.96 251.087 40.205 182.5h75.26c3.384 27.14 24.895 48.65 52.035 52.036v75.26z"/>

      <g v-if="currentLevel > 0">
        <g class="counter">
          <circle class="center" cx="175" cy="175.027" r="45"/>
          <g class="counter--pieces" :class="'counter--pieces__' + currentLevel">
            <path class="tens-2" d="M166.73 162.59l2.005 2.292-2.52 2.29h-14.89l-2.063-2.29 2.577-2.29z"/>
            <path class="tens-01" d="M169.823 165.455l1.03 1.145-1.66 16.036c-.535 1.528-1.585 2.482-3.15 2.864l-1.26-4.582 1.26-12.027 3.78-3.435z"/>
            <path class="tens-02" d="M164.897 185.5h-17.18c-1.49-.382-2.35-1.336-2.58-2.864l4.755-1.718h13.745l1.26 4.582zM145.253 181.49l1.546-14.89 1.26-1.145 3.092 3.436-1.145 10.883zM167.017 148.845h-13.69l-4.41-1.718c.536-1.527 1.586-2.482 3.15-2.863h17.126l-2.176 4.58z"/>
            <path class="tens-0" d="M153.157 149.99l-1.146 10.883-3.836 3.436-.974-1.146 1.547-14.89z"/>
            <path class="tens-012" d="M166.902 160.873l1.26-12.028 2.176-4.58c1.49.38 2.35 1.335 2.578 2.862l-1.66 16.037-1.32 1.145-3.034-3.437z"/>
            <path class="ones-2345689" d="M196.508 162.59l2.005 2.292-2.52 2.29h-14.89l-2.063-2.29 2.577-2.29z"/>
            <path class="ones-013456789" d="M199.6 165.455l1.032 1.145-1.66 16.036c-.536 1.528-1.586 2.482-3.15 2.864l-1.26-4.582 1.26-12.027 3.78-3.435z"/>
            <path class="ones-0235689" d="M194.675 185.5h-17.18c-1.49-.382-2.35-1.336-2.58-2.864l4.755-1.718h13.745l1.26 4.582z"/>
            <path class="ones-0268" d="M175.03 181.49l1.547-14.89 1.26-1.145 3.093 3.436-1.145 10.883z"/>
            <path class="ones-02356789" d="M196.795 148.845h-13.69l-4.41-1.718c.536-1.527 1.586-2.482 3.15-2.863h17.126l-2.175 4.58z"/>
            <path class="ones-0456789" d="M182.935 149.99l-1.146 10.883-3.838 3.436-.974-1.146 1.547-14.89z"/>
            <path class="ones-01234789" d="M196.68 160.873l1.26-12.028 2.176-4.58c1.49.38 2.35 1.335 2.578 2.862l-1.66 16.037-1.32 1.145-3.034-3.437z"/>
          </g>
        </g>
        <g class="btn btn--stop" @click="clearGame">
          <path class="center--semi" d="M217.072 191.027c-6.45 16.954-22.854 29-42.072 29-19.218 0-35.62-12.046-42.072-29h84.144z"/>
          <path class="stop--icon" d="M166.288 196.815h17.424v17.424h-17.424z"/>
        </g>
      </g>
      <g v-else>
        <g class="btn btn--play" @click="startGame">
          <circle class="center" cx="175" cy="175.027" r="45"/>
          <path class="play--icon" d="M163.035 140.9V184l33.93-21.55z"/>
        </g>
        <g class="btn btn--strict" :class="{'btn--strict__on': strictMode}" @click="strictMode = !strictMode">
          <path class="center--semi" d="M217.072 191.027c-6.45 16.954-22.854 29-42.072 29-19.218 0-35.62-12.046-42.072-29h84.144z"/>
          <g class="strict--text">
            <path d="M155.907 201.51l.573.654-.72.655h-4.255l-.59-.656.737-.654zM156.79 202.328l.295.327-.474 4.582c-.152.437-.452.71-.9.818l-.36-1.31.36-3.435 1.08-.982zM155.383 208.055h-4.91c-.424-.11-.67-.38-.735-.818l1.358-.49h3.927l.36 1.308zM152.03 197.91l-.33 3.11-1.095.98-.278-.326.442-4.255zM152.078 197.583l-1.26-.49c.153-.438.453-.71.9-.82h5.22c.425.11.67.382.736.82l-1.358.49h-4.238zM162.19 202.328l.59.655-.443 4.254c-.153.437-.453.71-.9.818l-.36-1.31.392-3.762.72-.655zM161.65 201.346l.36-3.436h1.31l-.36 3.436-.738.655zM160.586 197.583l-1.26-.49c.153-.438.453-.71.9-.82h5.22c.425.11.67.382.736.82l-1.358.49h-4.238zM172.923 201.51l.573.654-.72.655h-4.255l-.588-.656.736-.654zM168.816 203.146h.98l3.814 4.255c-.262.437-.72.655-1.374.655l-3.52-3.927.1-.982zM168.472 203.31l-.36 3.436-.622 1.31c-.425-.11-.67-.382-.736-.82l.474-4.58.36-.328.884.982zM173.005 197.583h-3.91l-1.26-.49c.152-.438.452-.71.9-.82h4.892l-.622 1.31zM172.972 201.02l.36-3.437.622-1.31c.425.11.67.382.736.82l-.474 4.58-.377.328-.868-.98zM169.045 197.91l-.328 3.11-1.096.98-.277-.326.442-4.255zM178.535 201.02l.36-3.437.62-1.31c.427.11.672.382.738.82l-.49 4.58-.36.328-.868-.98zM179.385 202.164l.295.328-.475 4.582c-.163.436-.47.71-.916.818l-.345-1.31.36-3.436 1.08-.982zM189.906 208.055h-5.4c-.425-.11-.67-.38-.736-.818l1.358-.49h4.418l.36 1.308zM183.803 206.91l.44-4.255.36-.327.885.982-.327 3.11zM190.512 197.583h-4.402l-1.26-.49c.153-.438.453-.71.9-.82h5.383l-.62 1.31zM186.06 197.91l-.327 3.11-1.096.98-.278-.326.44-4.255zM196.22 202.328l.59.655-.44 4.254c-.154.437-.454.71-.9.818l-.36-1.31.39-3.762.72-.655zM195.68 201.346l.36-3.436h1.31l-.36 3.436-.736.655zM194.618 197.583l-1.26-.49c.153-.438.453-.71.9-.82h5.22c.425.11.67.382.736.82l-1.358.49h-4.238z"/>
          </g>
        </g>
      </g>
    </svg>
    <!-- <button @click="currentLevel=20">to 20</button> -->
  </div>
</template>

<script>
import _ from 'lodash';
import Tone from 'tone';
var synth = new Tone.Synth().toMaster();

export default {
  data () {
    return {
      currentLevel: 0,
      levelArray: [],
      playerArray:[],
      gameArray: [],
      strictMode: false,
      sounds: ['E3','A4','E4','C4'],
      delayTimer: '',
      intervalTimer: ''
    }
  },
  methods: {
    startGame(){
      var sampleArray = [1,2,3,4];
      var currentSample;
      sampleArray = _.sampleSize(sampleArray, 3);
      do {
        var lastItems = sampleArray.slice(-3).join('');
        currentSample = _.random(1,4);
        if (lastItems !== _.repeat(''+currentSample, 3)) {
          sampleArray.push(currentSample);
        }
      } while (sampleArray.length < 20);
      this.gameArray = sampleArray;
      this.currentLevel = 1;
    },
    clearGame(){
      this.currentLevel= 0;
      this.levelArray= [];
      this.playerArray=[];
      this.gameArray= [];
      clearInterval(this.intervalTimer);
      clearInterval(this.delayTimer);
    },
    addLight(num){
      var el = document.querySelector(`.btn--${num}`);
      var className = `btn--${num}__pressed`;

      if (el.classList)
        el.classList.add(className);
      else
        el.className += ' ' + className;

      setTimeout(() => {
        if (el.classList)
          el.classList.remove(className);
        else
          el.className = el.className.replace(new RegExp('(^|\\b)' + className.split(' ').join('|') + '(\\b|$)', 'gi'), ' ');
      },250);
    },
    addSound(num){
      synth.triggerAttackRelease(this.sounds[num-1], '16n', '+0.1');
    },
    humanPlay(num){
      this.addLight(num);
      this.addSound(num);
      this.checkPlay(num);
    },
    checkPlay(num){
      var playLength = this.playerArray.length;
      console.log(this.playerArray);
      console.log(this.levelArray);
      if (this.levelArray[playLength] === num && this.levelArray.length-1 !== playLength) {
        console.log('correct');
        this.playerArray.push(num);
      } else if (this.levelArray[playLength] === num && this.levelArray.length-1 === playLength) {
        console.log('two');
        this.playerArray.push(num);
        this.currentLevel === 20 ? this.winMsg() : this.currentLevel++;
      } else if (this.strictMode) {
        console.log('strict');
        this.errorMsg();
        this.delayTimer = setTimeout(() => {
          this.currentLevel= 0;
          this.levelArray= [];
          this.playerArray=[];
          this.gameArray= [];

          this.startGame();
        },700);
      } else {
        console.log('nope');
        this.errorMsg();
        this.delayTimer = setTimeout(() => {
          this.samplePlay();
          this.playerArray = [];
        },700);
      }
    },
    samplePlay(){
      var seqNum = 0;
      var speed = 700;
      if (this.currentLevel > 12) {
        speed = 300;
      } else if (this.currentLevel > 7) {
        speed = 500;
      }
      this.delayTimer = setTimeout(() => {
        this.intervalTimer = setInterval(() => {
          if (seqNum === this.levelArray.length) {clearInterval(this.intervalTimer);return};
          var num = this.levelArray[seqNum];
          this.addSound(num);
          this.addLight(num);
          seqNum++;
        },speed); // speed should be 300, 500, 700 (easy to hard)
      },700);
    },
    errorMsg(){
      var seqNum = 0;
      var errSeq = [1,2,3,4];
      this.intervalTimer = setInterval(() => {
        if (seqNum === 4) {clearInterval(this.intervalTimer);return};
        var num = errSeq[seqNum];
        this.addSound(num);
        this.addLight(num);
        seqNum++;
      },50);
    },
    winMsg(){
      var seqNum = 0;
      var winSeq = [1,2,3,4,4,3,2,1];
      this.intervalTimer = setInterval(() => {
        if (seqNum === 8) {
          this.delayTimer = setTimeout(() => {
            this.clearGame();
          },500);
          return;
        };
        var num = winSeq[seqNum];
        this.addSound(num);
        this.addLight(num);
        seqNum++;
      },50);
    }
  },
  watch: {
    currentLevel(value){
      if (value !== 0) {
        this.playerArray = [];
        this.levelArray = this.gameArray.slice(0,value);
        this.samplePlay();
      }
    }
  }
}
</script>

<style lang="scss">
$steel: #666666;
$fuscous-gray: #3b3b3b; //dark grey
$magnesium: #b3b3b3; // light grey
$chelsea-cucumber: #919956; // green
$deep-chestnut: #b35049; // red
$marigold: #b3873d; // yellow
$paradiso: #467f80; // blue

$base-colors: $chelsea-cucumber, $deep-chestnut, $paradiso, $marigold;


#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  // margin-top: 60px;
  //
  position: absolute;
  top: 0;right: 0;bottom: 0;left: 0;
}

svg{
  width: 100%;height: 100%;
}

.base {
  fill: $fuscous-gray;
}
.shadow {
  fill-opacity: .1;
}

.btn{
  cursor: pointer;


  @for $i from 1 through 4 {
    &--#{$i} {
      fill: nth($base-colors, $i);

      &__pressed {
        fill: adjust-color(nth($base-colors, $i), $lightness:20%, $saturation: 30%);
        filter:url(#dropshadow);
      }

    }
  }

  &--play{
    .center{fill: $magnesium;}
    .play--icon{fill:$steel;}

    &:hover{
      .play--icon{fill:#fff;}
    }
  }

  &--strict{
    .center--semi{fill:$steel;}
    .strict--text{fill:$magnesium}

    &:hover{
      .strict--text{fill:#fff;}
    }

    &__on{
      .strict--text{fill:#fff;}
    }
  }

  &--stop{
    .center--semi{fill:$steel;}
    .stop--icon{fill:$magnesium}

    &:hover{
      .stop--icon{fill:#fff;}
    }
  }

}

.counter {
  .center {
    fill: $magnesium;
  }

  &--pieces {
    fill:#ccc;

    @for $i from 1 through 20 {
      $ones:0;$tens:x; // use nonsense number for `$tens`
      @if $i > 9 {
        $ones: str_slice("" + $i, 2);
        $tens: str_slice("" + $i, 0,1);
      } @else {
        $ones: $i;
      }
      &__#{$i} {
        [class^='ones-'][class*='#{$ones}']{fill:#000;}
        [class^='tens-'][class*='#{$tens}']{fill:#000;}
      }
    }
  }
}

</style>
