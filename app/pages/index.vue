<template>
  <div class="main">
    <!-- ---------------------------------------------------------------------- -->
    <!-- Intro Text -->
    <img src="/images/banner.JPG" class="banner" />
    <h1>Click <span class="arisTilt">Aris!</span></h1>
    <p>
      Click the <span class="arisTilt">Aris</span> head to collect more
      <span class="arisTilt">Aris!</span>
    </p>

    <!-- ---------------------------------------------------------------------- -->
    <!-- Aris' Head -->
    <div class="centered-content">
      <button
        @pointerdown="clickAris()"
        @click.stop="removeClasses(headAmountClass)"
        class="headbutton"
      >
        <img src="/images/arishead.png" class="arishead" />
      </button>
    </div>
    <br />
    <div class="headAmount">
      <p :class="headAmountClass">{{ HEADS.toLocaleString() }}</p>
    </div>

    <hr />

    <!-- ---------------------------------------------------------------------- -->
    <!-- Clicking Power -->
    <div class="centered-content">
      <p>Clicking power:</p>
      <p :class="clickPowerClass">{{ CLICKPOWER.toLocaleString() }} &nbsp;</p>
      <button
        @pointerdown="
          upgradeClickPower(
            purchase(
              HEADS,
              headAmountClass,
              clickUCost,
              clickPowerClass,
              clickPriceClass
            )
          )
        "
        @click.stop="
          removeClasses(clickPowerClass, clickPriceClass, headAmountClass)
        "
      >
        Upgrade&ensp;&nbsp;[
        <span :class="clickPriceClass">{{ clickUCost.toLocaleString() }}</span>
        ]
      </button>
    </div>

    <hr />

    <!-- ---------------------------------------------------------------------- -->
    <!-- Sulfur5 -->
    <div :class="level2">
      <div class="centered-content">
        <p>Sulfur5!</p>
        <p :class="sulfurCountClass">{{ SULFURS.toLocaleString() }} &nbsp;</p>
        <button
          @pointerdown="
            buySulfur(
              purchase(
                HEADS,
                headAmountClass,
                sulfurCost,
                sulfurCountClass,
                sulfurPriceClass
              )
            )
          "
          @click.stop="
            removeClasses(sulfurCountClass, sulfurPriceClass, headAmountClass)
          "
        >
          Buy!&ensp;&nbsp;[
          <span :class="sulfurPriceClass">{{
            sulfurCost.toLocaleString()
          }}</span>
          ]
        </button>
        <br />
      </div>
      <div style="display: flex; flex-wrap: nowrap; overflow-x: auto">
        <div
          class="nowrap"
          v-for="sulfurImg in sulfurImages"
          :key="sulfurImg.id"
        >
          <img :src="sulfurImg.src" class="sulfur" />
        </div>
      </div>
    </div>

    <hr :class="level2" />

    <!-- ---------------------------------------------------------------------- -->
    <!-- COUNTRY -->
    <div :class="level3">
      <div class="centered-content">
        <p>Road Country:</p>
        <p :class="countryCountClass">
          {{ COUNTRIES.toLocaleString() }} &nbsp;
        </p>
        <button
          @pointerdown="
            buyCountry(
              purchase(
                SULFURS,
                sulfurCountClass,
                countryCost,
                countryCountClass,
                countryPriceClass
              )
            )
          "
          @click.stop="
            removeClasses(
              countryCountClass,
              countryPriceClass,
              sulfurCountClass
            )
          "
        >
          Buy!&ensp;&nbsp;[
          <span :class="countryPriceClass">
            {{ countryCost.toLocaleString() }}</span
          >
          ]
        </button>
        <p>(sulfurs)</p>
        <br />
      </div>
      <div style="display: flex; flex-wrap: nowrap; overflow-x: auto">
        <div
          class="nowrap"
          v-for="countryImg in countryImages"
          :key="countryImg.id"
        >
          <img :src="countryImg.src" class="country" />
        </div>
      </div>
    </div>

    <hr :class="level3" />

    <!-- ---------------------------------------------------------------------- -->
    <!-- SUPER SECRET CHEAT ABILITY (toggle the 'hidden' class before push to main)-->
    <div class="hidden">
      <button
        @click="
          () => {
            HEADS += 10000;
          }
        "
      >
        Cheat!
      </button>
    </div>
  </div>
</template>

<script setup>
// ----------------------------------------------------------------
// imports
//-----------------------------------------------------------------
import { ref, watch } from "vue";

// amount of main currency the player has
let HEADS = ref(0);

// ----------------------------------------------------------------
// Clicking Effects
//-----------------------------------------------------------------
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

// ----------------------------------------------------------------
// Aris Head
//-----------------------------------------------------------------
let headAmountClass = ref([]);
function headClickDown() {
  HEADS.value += CLICKPOWER.value;
}
function clickAris() {
  headClickDown();
  yayFXstart(headAmountClass.value);
}

// ----------------------------------------------------------------
// Purchase function
//-----------------------------------------------------------------
//      quantityClass is how many of the purchased you currently have
function purchase(currency, currencyClass, cost, quantityClass, priceClass) {
  // if you cant afford it
  if (cost > currency) {
    sadFXstart(priceClass);
    sadFXstart(currencyClass);
    return false;
    // if you can afford it
  } else {
    yayFXstart(quantityClass);
    sadFXstart(currencyClass);
    return true;
  }
}

// ----------------------------------------------------------------
// Level 2
//-----------------------------------------------------------------
//      Hides content until you get 200 HEADS.
let level2 = ref("hidden");

watch(HEADS, (newValue, oldValue) => {
  if (newValue >= 200) {
    level2.value = "";
  }
});

// ----------------------------------------------------------------
// Level 3
//-----------------------------------------------------------------
//      Hides content until you get 1500 HEADS.
let level3 = ref("hidden");
watch(HEADS, (newValue, oldValue) => {
  if (newValue >= 1500) {
    level3.value = "";
  }
});

// ----------------------------------------------------------------
// Upgrade Click Power
//-----------------------------------------------------------------
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

// ----------------------------------------------------------------
// Sulfur5
//-----------------------------------------------------------------
let SULFURS = ref(0);
let sulfurLoops = ref([]);
let sulfurCost = computed(() => {
  return Math.round(1000 * (3 / 2) ** SULFURS.value);
});
function buySulfur(bool) {
  if (bool) {
    // subtr money first, otherwise price changes too
    HEADS.value -= sulfurCost.value;
    SULFURS.value += 1;
    newSulfur();
  }
}
function newSulfur() {
  sulfurImages.value.push({ id: SULFURS.value, src: "/images/sulfur5.png" });
  // watch amount of sulfurs to spend em if player buys countries.
  sulfurLoops.value.push(
    setInterval(() => {
      HEADS.value += 10;
    }, 1000)
  );
  console.log("made a new sulfur");
}
// function to remove sulfur loops and images if amount changes; run it right away to always monitor sulfur count.
function watchSulfurs() {
  watch(SULFURS, (newValue, oldValue) => {
    while (sulfurLoops.value.length > SULFURS.value) {
      console.log("losing sulfur!");
      clearInterval(sulfurLoops.value.pop());
      sulfurImages.value.pop();
    }
  });
}
watchSulfurs();
//     Sulfur styles
let sulfurCountClass = ref([]);
let sulfurPriceClass = ref([]);
//     Sulfur Images
let sulfurImages = ref([]);

// ----------------------------------------------------------------
// Road Country
//-----------------------------------------------------------------
let COUNTRIES = ref(0);
let countryCost = computed(() => {
  return 1 + COUNTRIES.value;
});
function buyCountry(bool) {
  if (bool) {
    // subtr money first, otherwise price changes too!
    SULFURS.value -= countryCost.value;
    newCountry();
    COUNTRIES.value += 1;
    setInterval(() => {
      HEADS.value += 500;
    }, 1000);
  }
}
function newCountry() {
  countryImages.value.push({
    id: COUNTRIES.value,
    src: "/images/roadcountry.jpg",
  });
}
// Country styles
let countryCountClass = ref([]);
let countryPriceClass = ref([]);
// Country Images
let countryImages = ref([]);
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
.nowrap {
  white-space: nowrap;
}
/* cosemetic style to tilt the word 'aris' */
.arisTilt:hover {
  display: inline-block;
  transition: 0.1s ease-in-out;
  transform: rotate(5deg);
}
/* Level Conent Hiding*/
.hidden {
  display: none;
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
/* Sulfur */
.sulfur {
  display: inline-block;
  height: 40vw;
  max-height: 300px;
}
/* Country */
.country {
  display: inline-block;
  height: 38vw;
  max-height: 290px;
  padding: 5px;
}
</style>
