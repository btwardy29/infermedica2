<template>
  <div id="nav">
    <div class="navigationBar">
      <img src="./assets/rick_morty_PNG7.png" />
      <div class="linksWrapper">
      <router-link to="/">New Message</router-link>
      <router-link to="/history">History</router-link>
      </div>
    </div>
    <div class="navbar"/>
  </div>
  <div class="container">
    <router-view />
  </div>
</template>

<style lang="scss">
@import './assets/variables.scss';

.navigationBar {
  width: 100%;
  height: 98px;
  position: relative;
  border-bottom: 24px solid $primary-color;
  
  img {
    position: absolute;
    top: 50%;
    left: 20vw;
    width: 144px;
    transform: translateY(-50%);
  }
}

.linksWrapper {
  display: flex;
  position: absolute;
  top: 36px;
  right: calc(10vw);
  align-items: center;
  justify-content: center;
}

a {
  padding: 10px 16px;
}

a.router-link-exact-active {
  background: $primary-color;
}

.container {
  min-width: 390px;
  max-width: 500px;
  margin: 0 auto;
  height: auto;
  position: relative;
  border: 20px solid transparent
}

textarea {
  border: 1px solid $primary-gray;
  border-radius: 8px;
  padding: 12px 16px;
  font-size: 14px;
  outline: none;
  width: 100%;
  display: block;
  font-family: $primary-font;
  resize: none;
  min-height: 140px;
}

@media screen and (max-width: 600px) {
  .linksWrapper {
    right: 10px;
  }
  .navigationBar {
    img {
      left: 50px;
      width: 110px;
    }
  }

}


</style>

