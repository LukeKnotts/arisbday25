<template>
  <div class="main">
    <img src="/images/banner.JPG" class="banner" />
    <h1>Click Aris!</h1>
    <p>
      Hello, Aris! I need some more sample text in here, but I dont want to use
      lorem ipsum. But when will it wrap???? When will it warp? I mean when will
      it rap?
    </p>
    <p>{{ cakes }}</p>
  </div>
</template>

<script setup>
import { ref } from "vue";
let cakes = ref(0);
</script>

<style scoped>
.banner {
  width: 100%;
  height: 160px;
  padding: 0px;
}
</style>
