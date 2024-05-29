<template>
    <div class="game_over">
      <div class="user_score">
        Ваш счёт:
        <!-- {{ score }} -->
      </div>
      <div class="buttons">
        <router-link to="/game">
          <PixelButton class="main-butn">
            ИГРАТЬ
          </PixelButton>
        </router-link>
        <router-link to="/leaderboard">
          <PixelButton class="main-butn">
            ЛИДЕРБОРД
          </PixelButton>
        </router-link>
        <router-link to="/">
          <PixelButton>
            МЕНЮ
          </PixelButton>
        </router-link>
      </div>
    </div>
  </template>
  
  <script>
  import { RouterLink } from "vue-router";
  import PixelButton from "../components/FancyButton.vue";
  
  export default {
    components: {
      PixelButton,
    }
  }
  </script>
  
  <style scoped>
  .game_over {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100vh;
  }
  
  .user_score {
    color: #ffffff;
    margin-bottom: 100px;
    font-size: 24px;
  }
  
  .buttons {
    display: flex;
    flex-direction: column;
  }
  
  .main-butn {
    margin-bottom: 2px;
  }
  
  </style>
  