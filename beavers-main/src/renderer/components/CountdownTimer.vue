<template>
  <div>
    <div class="countdown">
      <h1>Время: {{ formattedTime }}</h1>
      <!-- <h2>Счёт тест: {{ userscore }}</h2> -->
    </div>
    <div v-if="isPaused" class="pause-overlay">
      <div class="pause-menu">
        <h1>Пауза</h1>
        <div class="button-container">
          <PixelButton @click="togglePause">ПРOДOЛЖИТЬ</PixelButton>
          <PixelButton @click="restartGame">НАЧАТЬ С НАЧАЛА</PixelButton>
          <PixelButton @click="goToMenu">ВЫЙТИ В МЕНЮ</PixelButton>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import mitt from 'mitt';
import { eventBus } from '../App.vue';

export default {
  props: {
    userscore: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      endTime: Date.now() + 20000 + 2000, // 10 seconds in milliseconds
      intervalId: null,
      formattedTime: '00:00', // Initial formatted time
      isPaused: false,
      remainingTime: 18000,
      userscore: 0,
    };
  },
  mounted() {
    document.addEventListener('keydown', this.handleKeydown);
    this.startTimer();
    eventBus.on('increaseTimer', this.increaseTimer);
  },
  beforeUnmount() {
    clearInterval(this.intervalId);
    document.removeEventListener('keydown', this.handleKeydown);
    eventBus.off('increaseTimer', this.increaseTimer);
  },
  methods: {
    
    
    startTimer() {
      this.intervalId = setInterval(() => {
        if (!this.isPaused) {
          const now = Date.now();
          const distance = this.endTime - now;
          if (distance < 0) {
            clearInterval(this.intervalId);

            // Emit the event to signal the parent component that the timer has ended
            eventBus.emit('timerEnded');
            console.log('sms отправлено');
            console.log(userscore.value); // Emit the event to signal the parent component
          } else {
            this.remainingTime = distance;
            this.formattedTime = this.formatTime(distance);
          }
        }
      }, 1000);
    },
    formatTime(ms) {
      const minutes = Math.floor(ms / (1000 * 60));
      const seconds = Math.floor((ms % (1000 * 60)) / 1000);
      return `${minutes}:${seconds.toString().padStart(2, '0')}`;
    },
    handleKeydown(e) {
      if (e.key === 'Escape') {
        this.togglePause();
      }
    },
    togglePause() {
      this.isPaused = !this.isPaused;
      if (this.isPaused) {
        clearInterval(this.intervalId);
      } else {
        this.endTime = Date.now() + this.remainingTime;
        this.startTimer();
      }
    },
    goToMenu() {
      // Logic to navigate to the menu
      this.$router.push({ name: 'Menu' }); // Assuming you have a route named 'Menu'
    },
    restartGame() {
      // Logic to restart the game
      this.$router.go(0); // Reloads the current page
    },
    handleLevelChange() {
      // Increase the timer value by 5000
      this.distance += 5000;
    },
    increaseTimer() {
      this.remainingTime += 5000;
      this.endTime = Date.now() + this.remainingTime;
    },
  }
};
</script>

<style scoped>
h1 {
  color:#ffffff;
}
.countdown {
  text-align: center;
  font-size: 12px;
  margin-top: 10px;
}

.pause-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 999;
}

.pause-menu {
  padding: 20px;
  border-radius: 10px;
  text-align: center;
}

.pause-menu h1 {
  margin-bottom: 20px;
}

.button-container {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.button-container PixelButton {
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  border: 1px solid #b85fac;
  color: #ffffff;
}


</style>
