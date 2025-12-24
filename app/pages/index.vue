<template>
  <div class="main">
    <img src="/images/banner.JPG" class="banner" />
    <h1>Click Aris!</h1>
    <p>Click the Aris head to collect more Aris!</p>
    <div class="centered-content">
      <button @pointerdown="arisClickDown()" @click.stop="arisClickUp()">
        <img src="/images/arishead.png" class="image arishead" />
      </button>
    </div>
    <p :class="cakeAmountText">{{ cakes }}</p>
  </div>
</template>

<script setup>
import { ref } from "vue";
let cakes = ref(0);

let cakeAmountText = ref("cakeAmount");

function arisClickDown() {
  cakes.value += 1;
  cakeAmountText.value = "cakeAmount clickedText";
  console.log(cakeAmountText.value);
}
function arisClickUp() {
  console.log("hello?");
  cakeAmountText.value = "cakeAmount";
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
  transition: 0.1s ease-in;
}
.clickedText {
  color: rgb(84, 179, 227);
  transition: 0.1s ease-out;
  transform: scale(1.2);
}
.arishead {
  height: 40vw;
  max-height: 300px;

  padding: 2px;
  transition: 0.2s ease-out;

  outline: none;
}
.arishead:hover {
  transform: scale(1.02);
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
  transition: transform 0.2s ease-in-out;
}
button:active {
  background-color: transparent;
  transform: rotate(5deg) scale(1.02);
}
</style>
