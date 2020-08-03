<template>
  <div id="app">
    <div class="wrapper clearfix">

      <players
              v-bind:isWinner="isWinner"
              v-bind:scorePlayer="scorePlayer"
              v-bind:activePlayer="activePlayer"
              v-bind:currentScore="currentScore"/>

      <Controls
              v-bind:isplaying = "isPlaying"
              v-on:handleFinalScore = "handleFinalScore"
              v-bind:finalScore="finalScore"
              v-on:handleNewGame="handleNewGame"
              v-on:handleRollDice="handleRollDice"
              v-on:handleHold="handleHold"/>
      <Dices
              v-bind:dices = "dices"/>

      <popup-rule
              v-bind:isOpenPopup = "isOpenPopup"
              v-on:handleConfirm="handleConfirm"/>
    </div>
  </div>
</template>

<script>
  import players from "./components/players";
  import Controls from "./components/Controls";
  import Dices from "./components/Dices";
  import PopupRule from "./components/popupRule";
  export default {
  name: 'App',
    computed: {
      isWinner() {
        if (this.scorePlayer[0] >= this.finalScore || this.scorePlayer[1] >= this.finalScore) {
          // eslint-disable-next-line vue/no-side-effects-in-computed-properties
          this.isPlaying = false;
          return true;
        } else {
          return false;
        }
      }
    },
   data(){
    return{
      scorePlayer: [0, 0],
      currentScore: 0,
      activePlayer: 1,
      isPlayer: 0,
      isOpenPopup: false,
      isPlaying: false,
      dices: [1,5],
      finalScore: 10
    }
   },
    methods: {
      handleFinalScore(e){
        let number = parseInt(e.target.value);
        if (isNaN(number)){
          this.finalScore = '';
        }else {
          this.finalScore = number;
          console.log(this.finalScore);
        }
      },
    handleHold(){
        if (this.isPlaying){
          let scorePlayer = this.scorePlayer[this.activePlayer] + this.currentScore;
          this.$set(this.scorePlayer, this.activePlayer, scorePlayer);
          if (!this.isWinner()){
            this.nextPlayer();
          }
        }else {
          alert('please create new game!');
        }
      },
    nextPlayer(){
      this.activePlayer = this.activePlayer === 0 ? 1 : 0;
      setTimeout( () => {
        this.currentScore = 0;
      }, 100);
    },
      handleNewGame(){
      this.isOpenPopup = true;
      console.log(this.isOpenPopup);
      console.log(this.isOpenPopup);
    },
      handleConfirm(){
        this.isPlaying = true;
        this.isOpenPopup = false;
        this.currentScore = 0;
        this.scorePlayer =  [0, 0];
        console.log('get request confirm');
      },
      handleRollDice(){
        if (this.isPlaying){
          let dice1 = Math.floor(Math.random()*6) + 1;
          let dice2 = Math.floor(Math.random()*6) + 1;
          this.dices = [dice1, dice2];
          if (dice1 === 1 || dice2 === 1){
            setTimeout( () => {
              alert('your dice is 1. Your gold ' + this.currentScore + '. Game over!');
            }, 100);
            this.nextPlayer();
          }else {
            this.currentScore += this.currentScore + dice1 + dice2;
          }
        }else {
          alert('please click new game!');
        }
        console.log('recipe request roll dice');
      }
    },
    components: {
      PopupRule,
      Dices,
      players,
      Controls
  }
}
</script>

<style>

  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  .clearfix::after {
    content: "";
    display: table;
    clear: both;
  }

  body {
    background-image: linear-gradient(rgba(62, 20, 20, 0.4), rgba(62, 20, 20, 0.4)), url(./assets/back.jpg);
    background-size: cover;
    background-position: center;
    font-family: Lato;
    font-weight: 300;
    position: relative;
    height: 100vh;
    color: #555;
  }

  .wrapper {
    width: 1000px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #fff;
    box-shadow: 0px 10px 50px rgba(0, 0, 0, 0.3);
    overflow: hidden;
  }







</style>
