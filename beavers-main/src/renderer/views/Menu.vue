<template>
    <div class="menu">
      <div class="header">
        Меню
      </div>
      <div class="buttons">
        <router-link to="/game">
          <PixelButton>
            Играть
          </PixelButton>
        </router-link>
        <router-link to="/profile">
          <PixelButton>
            Профиль
          </PixelButton>
        </router-link>
        <router-link to="/leaderboard">
          <PixelButton>
            Лидерборд
          </PixelButton>
        </router-link>
        <div v-if="!isAuth">
          <router-link to="/login">
            <PixelButton>
              Войти
            </PixelButton>
          </router-link>
        </div>
        <div v-if="!isAuth">
          <router-link to="/register">
            <PixelButton>
              Регистрация
            </PixelButton>
          </router-link>
        </div>
        <div class="exit">
          <router-link to="/login">
            <PixelButton @click="handleExit">
              Выйти
            </PixelButton>
          </router-link>
        </div>
      </div>
    </div>
  </template>
  
  <script lang="ts">
  import { defineComponent } from "vue";
  import PixelButton from "../views/FancyButton.vue";
  
  export default defineComponent({
    components: {
      PixelButton
    },
    data() {
      let isAuth: boolean = false;
      return {
        isAuth
      };
    },
    async mounted() {
      this.checkAuth();
    },
    methods: {
      async handleExit() {
        localStorage.removeItem('token');
      },
      async checkAuth() {
        if (localStorage.getItem('token'))
          this.isAuth = true;
      }
    }
  });
  </script>
  
  <style lang="css" scoped>
  .menu {
    display: flex;
    flex-direction: column;
  }
  
  .leftSide {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 40%;
  }
  
  .info {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 60%;
  }
  
  .about {
    height: 45vh;
  }
  
  .game-route {
    height: 55vh;
    align-self: flex-start;
  }
  
  .buttons {
    display: flex;
    flex-direction: column;
  }
  
  .exit {
    position: absolute;
    bottom: 2em;
  }
  
  .btns {
    width: 9em;
    font-size: 4cqmin;
    margin-bottom: 0.5em;
  }
  
  .header {
    font-size: 30px;
    text-align: center;
    padding: 20px;
    color: #7f9e9f;
    font-weight: 700;
  }
  </style>
  