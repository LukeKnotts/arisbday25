<template>
  <div class="main">
    <img src="/images/banner.JPG" class="banner" />
    <h1>Click Aris!</h1>
    <p>Click the Aris head to collect more Aris!</p>

    <div class="centered-content">
      <button
        @pointerdown="
          () => {
            headClickDown();
            yayFXstart(headAmountClass);
          }
        "
        @click.stop="removeFX(headAmountClass)"
        class="headbutton"
      >
        <img src="/images/arishead.png" class="arishead" />
      </button>
    </div>
    <br />
    <p :class="headAmountClass">{{ cakes }}</p>
    <hr />

    <div>
      <!-- "uClickClass"   Upgrade Clicking Power -->
      <div class="centered-content">
        <p>Clicking power:</p>
        <p :class="uClickClass">{{ clickPower }} &nbsp;</p>
        <button
          @pointerdown="
            () => {
              upgradeClickPower();
            }
          "
          @click.stop="
            () => {
              removeFX(uClickClass);
              removeFX(headAmountClass);
            }
          "
        >
          Upgrade [{{ clickUCost }}]
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
let cakes = ref(0);

let uClickClass = ref(["clickUButton"]);

// Clicking Effects
function yayFXstart(ele) {
  ele.push("yayText");
}
function sadFXstart(ele) {
  ele.push("sadText");
}
function removeFX(ele) {
  ele.pop();
}

// Aris Head
let headAmountClass = ref(["cakeAmount"]);

function headClickDown() {
  cakes.value += clickPower.value;
}

// Upgrade Click power
let clickPower = ref(1);
let clickUCost = computed(() => {
  return 30 * 2 ** clickPower.value - 30;
});
function upgradeClickPower() {
  if (cakes.value >= clickUCost.value) {
    // subtract money first, otherwise clickUCost is updated to new price
    cakes.value -= clickUCost.value;
    clickPower.value += 1;
    yayFXstart(uClickClass.value);
    sadFXstart(headAmountClass.value);
  } else {
    sadFXstart(uClickClass.value);
    sadFXstart(headAmountClass.value);
  }
}
</script>

<style scoped>
/* Decorative Styles */
.banner {
  width: 100vw;
  height: 18vw;
  min-height: 120px;

  padding: 0px;
  margin: 0px;
}
.centered-content {
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  width: 100%;
}
/* Cake Amount */
.cakeAmount {
  text-align: center;
  transition: 0.1s ease-in;
}
/* Generic Effects */
.yayText {
  color: rgb(84, 179, 227);
  transition: 0.1s ease-out;
  transform: scale(1.2);
}
.sadText {
  color: rgb(209, 37, 37);
  transition: 0.1s ease-out;
  transform: scale(1.2);
}
/* Aris Head */
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
.headbutton {
  width: 40%;
  background-color: transparent;
  border: none;
  transition: transform 0.2s ease-in-out;
}
.headbutton:active {
  background-color: transparent;
  transform: rotate(5deg) scale(1.02);
}
</style>
