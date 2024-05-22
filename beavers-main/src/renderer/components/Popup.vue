<template>
    <div class="popup" :class="{ 'fade-out': isFadingOut }" v-show="isVisible || isFadingOut">
      <div class="popup-content">
        <h1>Спаси село!</h1>
        <img src="../assets/letter.png" alt="Справка" class="help-image">
        <p>Победи весь нечистый духъ, да спокойно спать людъ добрый.</p>
      </div>
    </div>
  </template>
  
  <script lang="ts">
  import { defineComponent, watch } from 'vue';
  
  export default defineComponent({
    props: {
      isVisible: {
        type: Boolean,
        required: true
      }
    },
    data() {
      return {
        isFadingOut: false
      };
    },
    watch: {
      isVisible(newVal) {
        if (!newVal) {
          this.isFadingOut = true;
          setTimeout(() => {
            this.isFadingOut = false;
          }, 1000); // Длительность анимации исчезновения
        }
      }
    }
  });
  </script>
  
  <style scoped>
  .popup {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
    transition: opacity 1s ease-out;
  }
  
  .popup-content {
    background-color: #570850;
    background-size: cover;
    padding: 20px;
    text-align: center;
    border-radius: 8px;
  }
  
  .help-image {
    max-width: 100%;
    height: auto;
    margin: 20px 0;
  }
  
  h1 {
    font-size: 24px;
    margin-bottom: 10px;
    color: #fcfcfc;
  }
  
  p {
    font-size: 18px;
    color: #fcfcfc;
  }
  
  .fade-out {
    opacity: 0;
  }
  </style>
  