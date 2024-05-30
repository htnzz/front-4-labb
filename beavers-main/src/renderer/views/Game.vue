<template>
  <div>
    <Popup :isVisible="showPopup" />
    <div class="game" v-show="!showPopup">
      <Board />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Popup from '../components/Popup.vue';
import GameButton from "../components/GameButton.vue";
import Leaderboard from "../views/Leaderboard.vue";
import http from "../http_common";
import Board from "../components/Board.vue";
import { eventBus } from '../App.vue';

export default defineComponent({
  components: {
    GameButton,
    Leaderboard,
    Board,
    Popup,
  },
  data() {
    return {
      currScore: 0,
      counter: 20,
      score: 0,
      isLeftBeaver: true,
      isRightBeaver: false,
      isRightLog: true,
      isLeftLog: false,
      caught: false,
      user: null,
      userscore: 0,
      showPopup: true,
      isFadingOut: false,
      isPaused: false,
    };
  },
  methods: {
    handleSubmit() {
      if (this.score > this.userscore) {
        const response = http.put('/user/update/', { score: this.score });
      }
    },
    startGame() {
      setTimeout(() => {
        this.showPopup = false;
        }, 1000); // Длительность анимации исчезновения// Время до начала исчезновения
    },
  },
  async mounted() {
    await http
      .get('/user/')
      .then((response) => {
        this.user = response.data;
        this.userscore = response.data.score;
      })
      .catch((e) => {
        console.log(e);
      });
    this.startGame();
    eventBus.on('timerEnded', () => {
      this.$router.push({ name: 'GameOver' });
    });
  },
});
</script>

<style scoped>
.objects {
  display: flex;
  justify-content: space-evenly;
  width: 70%;
}

.game-over {
  display: flex;
  flex-direction: column;
  align-items: center;
  color: #2f1e1e;
  background-color: #b85fac;
  border-radius: 5px;
  font-size: 26px;
  padding: 40px;
  font-weight: 700;
}

.bounty-rune {
  width: 20%;
}

.btn {
  display: flex;
  justify-content: space-evenly;
  width: 50%;
  margin-top: 30px;
}

.game {
  text-align: center;
  margin-top: 30px;
  user-select: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
}

a {
  text-decoration: none;
}

.beaver-img {
  width: 50%;
}

.bg {
  display: flex;
  flex-direction: column;
  height: 100vh;
}

.logs-img {
  width: 96%;
  z-index: 1;
  position: absolute;
  left: 0;
  bottom: 0;
  transform: scale(1.08, 1);
}

.header {
  background-color: #060223;
  font-size: 30px;
  text-align: center;
  padding: 20px;
  color: #7f9e9f;
  font-weight: 700;
  z-index: 2;
}

.timer {
  font-size: 26px;
  padding: 30px;
  font-weight: 700;
}

.score-info {
  font-size: 22px;
  font-size: 26px;
  margin-bottom: 60px;
  font-weight: 700;
}

.pause-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 50px;
  z-index: 999;
}
</style>
