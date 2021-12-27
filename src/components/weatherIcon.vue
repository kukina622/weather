<template>
  <img width="120" :src="getWeatherIcon" />
</template>

<script>
export default {
  props: {
    weatherCode: {
      type: Number,
      default: 0
    }
  },
  computed: {
    getWeatherIcon() {
      let weatherIcon = undefined;
      switch (this.weatherCode) {
        case 1:
          weatherIcon = require("@/assets/weather/1.svg");
          break;
        case 2:
          weatherIcon = require("@/assets/weather/2.svg");
          break;
        case 3:
          weatherIcon = require("@/assets/weather/3.svg");
          break;
        case 4:
        case 5:
        case 6:
        case 7:
          weatherIcon = require("@/assets/weather/4.svg");
          break;
        default:
          weatherIcon = require("@/assets/weather/10.svg");
          break;
      }
      return weatherIcon;
    }
  }
};
</script>
