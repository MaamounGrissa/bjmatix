<template>
  <header>
      <img class="logo" alt="App Logo" src="./../assets/logo2.jpg">
      <p>Special Prices <br> For the best quality</p>
  </header>
</template>

<script>
export default {
  name: 'Header',
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
header {
    display: flex;
    align-items: center;
    max-width: 767px;
   /*  position: fixed;
    top: 0;
    left: calc(100% - 767px)/2; */
    z-index: 100;
    background: #338700;
    border-radius: 0 0 20px 20px;
    overflow: hidden;
    box-shadow: 0 20px 15px -10px rgb(0 0 0 / 40%);
}

.logo {
    width: 375px;
    box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
    margin-right: 30px;
}

p {
  font-family: 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
  font-size: 2rem;
  font-weight: 600;
  text-align: center;
  line-height: 1.4;
  color: black;
}

@media(max-width: 500px) {
  header {
    width: 100%;
    left: 0;
  }

  .logo {
    width: 170px;
    margin-right: 15px;
  }

  p {
    font-size: 1rem;
  }
}
</style>
