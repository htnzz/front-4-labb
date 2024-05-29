<template>
    <div class="leaderboard">
      <div class="header">Таблица лидеров</div>
      <div class="leaderboard-list">
        <div class="leaderboard-item" v-for="user in users" :key="user.id">
          <div>{{ user.username }}</div>
          <div>{{ user.score }}</div>
          <div>{{ formatTime(user.time) }}</div>
        </div>
      </div>
    </div>
  </template>
  
  <script lang="ts">
  import { defineComponent } from "vue";
  import http from "../http_common";
  
  export default defineComponent({
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
      await http.get('/users/').then((response) => {
        this.users = response.data;
      }).catch((e) => {
        console.log(e);
      });
    }
  });
  </script>
  
  <style scoped>
  .leaderboard {
    text-align: center;
  }
  
  .header {
    font-size: 30px;
    padding: 20px;
  }
  
  .leaderboard-list {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  
  .leaderboard-item {
    display: flex;
    justify-content: space-between;
    width: 50%;
    padding: 10px;
    border-bottom: 1px solid #ccc;
  }
  </style>
  