<template>
  <div>
    <NuxtPage />
  </div>
</template>

<style>
/* Disable doubel tap to zoom for mobile devices, and selecting text **/
* {
  touch-action: manipulation;

  -webkit-touch-callout: none; /* iOS Safari */
  -webkit-user-select: none; /* Safari, Chrome */
  -khtml-user-select: none; /* Konqueror HTML */
  -moz-user-select: none; /* Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
  user-select: none; /* Standard syntax */
}

/* STYLES */
h1 {
  font-size: 30px;
  margin: 5px;
}
p {
  font-size: 22px;
  margin: 5px;
}
body {
  margin: 0px;
  padding: 0px;
}
</style>
