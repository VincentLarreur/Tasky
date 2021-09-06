<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Ubuntu:wght@300;400;700&display=swap');

html {
  height: 100%;
  width: 100%;
  --dark-blue: #111736;
  --blue: #2C397F;
  --light-blue: #5864A7;
  --lighter-blue: #8494B4;
  background-image: linear-gradient(to left top, var(--dark-blue), #171f47, #1e2859, #25306c, var(--blue));
  font-family: "Ubuntu";
}
</style>
