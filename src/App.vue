<template>
  <img alt="Color Picker" src="./assets/logo.png">
  <BoxInfo title="Color picker" />
  <ColorPicker/>
  <FooterApp />
</template>

<script>
import ColorPicker from './components/ColorPicker.vue';
import BoxInfo from './components/BoxInfo.vue';
import FooterApp from './components/FooterApp.vue';
import 'bulma/css/bulma.css';

export default {
  name: 'App',
  components: {
    ColorPicker,
    BoxInfo,
    FooterApp,
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #ffff00;
  margin-top: 60px;
}
</style>
