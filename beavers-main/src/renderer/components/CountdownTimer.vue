<template>
  <div class="countdown">
    <h1>Время: {{ formattedTime }}</h1>
  </div>
</template>

<script>
import mitt from 'mitt';
import { eventBus } from '../App.vue'

export default {
  data() {
    return {
      endTime: Date.now() + 18000, // 10 seconds in milliseconds
      intervalId: null,
      formattedTime: '00:00' // Initial formatted time
    };
  },
  mounted() {
    this.startTimer();
  },
  beforeUnmount() {
    clearInterval(this.intervalId);
  },
  methods: {
    startTimer() {
      this.intervalId = setInterval(() => {
        const now = Date.now();
        const distance = this.endTime - now;

        if (distance < 0) {
          clearInterval(this.intervalId);

          // Assuming you have a parent component that listens for the 'timer-end' event
          eventBus.emit('timerEnded')
          console.log('sms отправлено'); // Emit the event to signal the pare

          // If using Vue Router for navigation (assuming it's set up):
           // Navigate to the other component
        } else {
          this.formattedTime = this.formatTime(distance);
        }
      }, 1000);
    },
    formatTime(ms) {
      const minutes = Math.floor(ms / (1000 * 60));
      const seconds = Math.floor((ms % (1000 * 60)) / 1000);
      return `${minutes}:${seconds.toString().padStart(2, '0')}`;
    }
  }
};
</script>

<style scoped>
.countdown {
  text-align: center;
  font-size: 18px;
}
</style>
