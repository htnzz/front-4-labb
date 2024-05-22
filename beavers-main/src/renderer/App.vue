<template>
  <div class="pixelify-sans">
    <router-view></router-view>
    <footer class="footer">
      beta v1.0.0
    </footer>
  </div>
</template>

<script setup lang="ts">

</script>

<style lang="css">
@import url('https://fonts.googleapis.com/css2?family=Pixelify+Sans:wght@400..700&display=swap');

body {
  margin: 0;
  padding: 0;
  font-family: 'Pixelify Sans', sans-serif;
  background-image: url('../assets/back3.png');
  background-size: auto;
  color: #7f9e9f;
}

#app {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  height: 100vh;
}

.pixelify-sans {
  font-family: 'Pixelify Sans', sans-serif;
  font-optical-sizing: auto;
  font-weight: 400; /* Adjust weight as needed */
  font-style: normal;
}

.footer {
  width: 100%;
  text-align: center;
  padding: 10px 0;
  color: #ffffff;
  position:fixed;
  bottom: 0;
  left: 0;
}
</style>
