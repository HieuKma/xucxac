<template>
  <div id="app">
    <div class="wrapper clearfix">
      <players
        :activePlayer="activePlayer"
        :scorePlayers="scorePlayers"
        :currentScore="currentScore"
      >
      </players>

      <controls
        :isPlaying="isPlaying"
        :finalScore="finalScore"
        v-on:handleChangeHandleScore="handleChangeHandleScore"
        v-on:handleNewgame="handleNewgame"
        v-on:handleRolldice="handleRolldice"
        v-on:handleHoldScore="handleHoldScore">
      </controls>

      <dices :dices="dices"></dices>

      <popup v-on:handleConfirm="handleConfirm" :isOpenPopup="isOpenPopup"> </popup>
    </div>
  </div>
</template>

<script>
import Players from "./components/Players";
import Controls from "./components/Controls";
import Dices from "./components/Dices";
import Popup from "./components/Popup";
export default {
  name: "app",
  data() {
    return {
      isPlaying: false,
      isOpenPopup: false,
      activePlayer: 0,
      scorePlayers: [20, 55],
      dices: [5, 3],
      currentScore: 30,
      finalScore: 100,
    };
  },
  components: {
    Players,
    Controls,
    Dices,
    Popup,
  },
  methods: {
    handleChangeHandleScore(e) {
      let number = parseInt(e.target.value);
      if(isNaN(number)) {
        this.finalScore = '';
      } else {
        this.finalScore = number;
      }
    },
    handleHoldScore() {
      if(this.isPlaying) {
        let { scorePlayers, activePlayer, currentScore } = this;
        let scoreOld = scorePlayers[activePlayer];
        // let cloneScorePLayer = [...scorePlayers];
        //     cloneScorePLayer[activePlayer] = scoreOld + currentScore;
        // this.scorePlayers = cloneScorePLayer;
        this.$set(this.scorePlayers, activePlayer, currentScore + scoreOld);
        this.nextPlayer();
      } else {
        alert('Click Newgame');
      }
    },
    nextPlayer() {
      this.activePlayer = this.activePlayer === 0 ? 1 : 0;
      this.currentScore = 0;
    },
    handleRolldice() {
      console.log("HandlerollDice tu App.vue");
      if(this.isPlaying) {
        let number1 = Math.floor(Math.random() * 6) + 1;
        let number2 = Math.floor(Math.random() * 6) + 1;
        this.dices = [number1, number2];

        if(number1 == 1 || number2 == 1) {
          let activePlayer = this.activePlayer + 1;
          setTimeout(() => {
            alert(`Người chơi Player ${activePlayer} đã quay trúng số 1. Rất tiếc`);
          }, 10);
          this.nextPlayer();
        } else {
          this.currentScore += number1 + number2;
        }
      } else {
        alert('Click Newgame');
      }
    },
    handleConfirm() {
      this.isPlaying = true;
      this.isOpenPopup = false;
      this.activePlayer = 0;
      this.scorePlayers = [0, 0];
      this.dices = [1, 1],
      this.currentScore = 0;
    },
    handleNewgame() {
      console.log("Handle NewGame tu App.vue");
      this.isOpenPopup = true;
    }
  },
};
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
  background-image: linear-gradient(rgba(62, 20, 20, 0.4), rgba(62, 20, 20, 0.4)),
    url("./assets/back.jpg");
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
