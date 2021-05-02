<template>
  <div id="app">
    <div class="wrapper clearfix">
      <players
        :activePlayer="activePlayer"
        :scorePalyers="scorePalyers"
        :currentScore="currentScore"
      >
      </players>

      <controls v-on:handleNewgame="handleNewgame"></controls>

      <dices :dices="dices"></dices>

      <popup
        v-on:handleConfirm="handleConfirm"
        :isOpenPopup="isOpenPopup">
      </popup>
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
      scorePalyers: [20, 55],
      dices: [5, 3],
      currentScore: 30,
    };
  },
  components: {
    Players,
    Controls,
    Dices,
    Popup,
  },
  methods: {
    handleNewgame() {
      console.log("Handle NewGame tu App.vue");
      this.isOpenPopup = true;
    },
    handleConfirm() {
      this.isPlaying = true;
      this.isOpenPopup = false;
      this.activePlayer = 0;
      this.scorePalyers = [0, 0];
      this.dices = [1, 1],
      this.currentScore = 0
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
