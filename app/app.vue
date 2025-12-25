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
:root {
  /* CSS variables so I can standardize font size and stuff. */
  --font-size: 22px;
  --heading-size: 30px;
}
h1 {
  font-size: var(--heading-size);
  margin: 5px;
  margin-left: 15px;
}
p {
  font-size: var(--font-size);
  margin: 5px;
  margin-left: 15px;
}
body {
  margin: 0px;
  padding: 0px;
}
button {
  font-size: calc(var(--font-size) - 2);
}
</style>
