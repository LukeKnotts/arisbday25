<template>
  <div class="main">
    <img src="/images/banner.JPG" class="banner" />
    <h1>Click Aris!</h1>
    <p>Click the Aris head to collect more Aris!</p>
    <div class="centered-content">
      <button @click="arisClick()">
        <img src="/images/arishead.png" class="image arishead" />
      </button>
    </div>
    <p class="cakeAmount">{{ cakes }}</p>
  </div>
</template>

<script setup>
import { ref } from "vue";
let cakes = ref(0);

function arisClick() {
  cakes.value += 1;
}
</script>

<style scoped>
.banner {
  width: 100vw;
  height: 18vw;
  min-height: 120px;

  padding: 0px;
  margin: 0px;
}
.cakeAmount {
  text-align: center;
}
.arishead {
  height: 40vw;
  max-height: 300px;

  padding: 2px;
  transition: 0.2s ease-out;
}
.arishead:hover {
  transform: scale(1.05);
  transition: 0.1s ease-out;
}
.centered-content {
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  width: 100%;
}
button {
  width: 40%;
  background-color: transparent;
  border: none;
  transition: 0.2s ease-out;
}
button:active {
  background-color: transparent;
  transform: rotate(5deg);
  transition: 0.2s ease-out;
}
</style>
