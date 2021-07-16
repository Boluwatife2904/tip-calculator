<template>
  <div class="container">
    <div class="logo">
      <img src="./assets/images/logo.svg" alt="Logo">
    </div>
    <div class="tip-calculator">

    </div>
  </div>
</template>

<script>
export default {
  name: "App",
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Space+Mono:ital,wght@0,400;0,700;1,400;1,700&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  scroll-behavior: smooth;
}

body {
  font-family: "Space Mono", sans-serif;
}

#app {
  height: 100vh;
  width: 100vw;
  display: flex;
  justify-content: center;
  align-items: center;
  background: hsl(185, 41%, 84%);

  .container {
    max-width: 800px;
    margin: 0 auto;
    width: 95%;

    .logo {
      img {
        display: block;
        margin: 0 auto 80px;
      }
    }

    .tip-calculator {
      background: hsl(0, 0%, 100%);
      padding: 100px;
      border-radius: 20px;
    }
  }
}
</style>
