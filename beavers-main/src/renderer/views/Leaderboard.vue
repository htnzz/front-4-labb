<template>
  <div class="leaderboard">
    <div class="header">Таблица лидеров</div>
    <div class="leaderboard-list">
      <div class="leaderboard-item" v-for="user in users" :key="user.id">
        <div class="username">{{ user.username }}</div>
        <div class="score">{{ user.score }}</div>
        <div class="time">{{ formatTime(user.time) }}</div>
      </div>
    </div>
    <router-link to="/" class="back-button">
      <PixelButton class="main-button">НАЗАД</PixelButton>
    </router-link>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import http from "../http_common";
import PixelButton from "../components/FancyButton.vue";

export default defineComponent({
  components: {
    PixelButton
  },
  data() {
    return {
      users: [] as Array<{ id: number, username: string, score: number, time: number }>
    };
  },
  methods: {
    formatTime(ms: number) {
      const seconds = Math.floor(ms / 1000) % 60;
      const minutes = Math.floor(ms / 60000);
      return `${minutes}:${seconds < 10 ? '0' : ''}${seconds}`;
    }
  },
  async mounted() {
    try {
      const response = await http.get('/users/');
      this.users = response.data;
    } catch (e) {
      console.log(e);
    }
  }
});
</script>

<style scoped lang="css">
.leaderboard {
  text-align: center;
  color: #fcfcfc;
  font-family: 'Pixelify Sans', sans-serif;
}

.header {
  font-size: 30px;
  padding: 20px;
  font-weight: 700;
}

.leaderboard-list {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 20px;
}

.leaderboard-item {
  display: flex;
  justify-content: space-between;
  width: 80%;
  padding: 10px 20px;
  margin-bottom: 10px;
  border: 3px solid #b85fac;
}

.username, .score, .time {
  flex: 1;
  text-align: center;
  font-size: 18px;
}

.back-button {
  display: inline-flex;
  justify-content: center;
  border: 2px solid #b85fac;
  position: fixed;
  bottom: 40px;
  left: 50%;
  transform: translateX(-50%);
}

</style>
