<template>
  <div id="app">
      <div class="container app-container">

      <h1 class="title is-1">Настройки светильника</h1>
      <h2 class="subtitle is-2">Выберите цвет</h2>
        <div class="color-picker-container mt-6">
          <ColorPicker :width="300" :height="300" :disabled="false" startColor="#ff0000" @colorChange="onColorChange"></ColorPicker>
          <div class="selected-color mt-6" :style="{backgroundColor: color}"></div>
          <h5 class="subtitle is-4 mt-4"> {{color}} </h5>
          <div class="info info-success has-background-success mt-4" v-if="!isLoading && requestSend">Изменеия применены</div>
        </div>
      </div>
  </div>
</template>

<script>
import ColorPicker from 'vue-color-picker-wheel';
import axios from 'axios';
import './../node_modules/bulma/css/bulma.css';
import debounce from 'lodash.debounce';
const API_URL = 'http://snufkin1514.pythonanywhere.com/manual/snufkin/set_led?'
export default {
  name: 'App',
  components: {
    ColorPicker
  },
  data () {
    return {
      color: '#ff0000',
      isLoading: false,
      requestSend: false
    }
  },
  methods: {
    onColorChange(color) {
      this.color = color
      this.isLoading = true;
      this.requestSend= false;
      this.applyColor(this, color)
    },
    applyColor: debounce(async (vi, color)=>{
        const response = await axios.get(API_URL, {
          params: {
            led_color: color
          }
        })
        vi.isLoading = false
        vi.requestSend = true
        console.log('responce', response.data)
      },500)
  }
}
</script>

<style>
.app-container {
  padding: 16px;
}
.color-picker-container{
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
.selected-color{
  width: 200px;
  height: 50px;
  background-color: red;
}
.info{
  padding-left: 16px;
  padding-right: 16px;
  padding-top: 8px;
  padding-bottom: 8px;
}
</style>
