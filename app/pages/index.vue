<template>
  <div class="main">
    <img src="/images/banner.JPG" class="banner" />
    <h1>Click Aris!</h1>
    <p>Click the Aris head to collect more Aris!</p>

    <!-- Aris' Head -->
    <div class="centered-content">
      <button
        @pointerdown="
          () => {
            headClickDown();
            yayFXstart(headAmountClass);
          }
        "
        @click.stop="removeClasses(headAmountClass)"
        class="headbutton"
      >
        <img src="/images/arishead.png" class="arishead" />
      </button>
    </div>
    <br />
    <div class="headAmount">
      <p :class="headAmountClass">{{ HEADS }}</p>
    </div>

    <hr />

    <!-- Clicking Power -->
    <div class="centered-content">
      <p>Clicking power:</p>
      <p :class="clickPowerClass">{{ CLICKPOWER }} &nbsp;</p>
      <button
        @pointerdown="
          upgradeClickPower(
            purchase(clickUCost, clickPowerClass, clickPriceClass)
          )
        "
        @click.stop="
          () => {
            removeClasses(clickPowerClass, clickPriceClass, headAmountClass);
          }
        "
      >
        Upgrade [<span :class="clickPriceClass">{{ clickUCost }}</span
        >]
      </button>
    </div>
  </div>
</template>

<script setup>
// imports
import { ref } from "vue";

// amount of 'currency' the player has
let HEADS = ref(0);

// Clicking Effects
function removeClasses(...targets) {
  targets.forEach((target) => {
    target.length = 0;
  });
}

// Click Effects
function yayFXstart(ele) {
  ele.push("yayText");
}
function sadFXstart(ele) {
  ele.push("sadText");
}

// Aris Head
let headAmountClass = ref([]);
function headClickDown() {
  HEADS.value += CLICKPOWER.value;
}

// Purchase function
//      quantityClass is how many of the purchased you currently have
function purchase(cost, quantityClass, priceClass) {
  if (cost > HEADS.value) {
    sadFXstart(priceClass);
    sadFXstart(headAmountClass.value);
    return false;
  } else {
    yayFXstart(quantityClass);
    sadFXstart(headAmountClass.value);
    return true;
  }
}

// Upgrade Click Power
let CLICKPOWER = ref(1);
let clickUCost = computed(() => {
  return 30 * 2 ** CLICKPOWER.value - 30;
});
function upgradeClickPower(bool) {
  if (bool) {
    // subtract money first, otherwise clickUCost is updated to new price
    HEADS.value -= clickUCost.value;
    CLICKPOWER.value += 1;
  }
}
//      Click Power styles
let clickPowerClass = ref([]);
let clickPriceClass = ref([]);
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
/* Head Amount */
.headAmount {
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
