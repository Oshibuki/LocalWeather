<template>
  <div class="localWeather">
    <h1 class="title">
      Free C<img id="weatherIcon" :srcset="weatherIconUrl" />de Camp
    </h1>
    <h1 class="title">Weather App</h1>
    <h3 id="location">{{city+", "+nation}}</h3>
    <h3 id="temperature" v-cloak>
      <span id="tempNum">{{temperatureUnit==="Celsius"?temperatureCelsius:temperatureFahrenheit}}</span>
      <span
        id="tempUnit"
        @click="temperatureUnit=temperatureUnit==='Celsius'?'Fahrenheit':'Celsius'"
        v-cloak
      >{{temperatureUnit==="Celsius"?"&#8451;":"&#8457;"}}</span>
    </h3>
    <h3 id="weather">{{weather}}</h3>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "LocalWeather",
  data() {
    return {
      weather: "",
      nation: "",
      city: "",
      temperatureCelsius: 0,
      temperatureUnit: "Celsius",
      weatherIconUrl: ""
    };
  },
  computed: {
    temperatureFahrenheit(){
      return ((this.temperatureCelsius * 9) / 5 + 32).toFixed(2);
    }
  },
  mounted() {
    this.getWeatherInfo();
  },
  methods: {
    getWeatherInfo() {
      let that = this;
      if ("geolocation" in navigator) {
        /* geolocation is available */
        var options = {
          enableHighAccuracy: true,
          timeout: 5000,
          maximumAge: 0
        };

        function success(pos) {
          var crd = pos.coords;

          console.log("Your current position is:");
          console.log("Latitude : " + crd.latitude);
          console.log("Longitude: " + crd.longitude);
          console.log("More or less " + crd.accuracy + " meters.");
          let url = `https://fcc-weather-api.glitch.me/api/current`;
          axios
            .get(url, {
              params: {
                lat: crd.latitude,
                lon: crd.longitude
              }
            })
            .then(that.getWeatherInfoSucc);
        }

        function error(err) {
          console.warn("ERROR(" + err.code + "): " + err.message);
        }

        navigator.geolocation.getCurrentPosition(success, error, options);
      } else {
        /* geolocation IS NOT available */
        alert("无法获得位置信息，无法提供服务");
      }
    },
    getWeatherInfoSucc(res) {
      res = res.data;
      this.temperatureCelsius = res.main.temp;
      this.nation = res.sys.country;
      (this.city = res.name),
        (this.weather = res.weather[0].main),
        (this.weatherIconUrl = res.weather[0].icon);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h1.title {
  font-size: 6rem;
}

h1.title:first-of-type:before {
  content: "";
  display: inline-block;
  height: 1px;
}

img#weatherIcon {
  height: 4.5rem;
  vertical-align: middle;
}

h3 {
  margin: 2.5rem 0 0;
  font-size: 3rem;

  span#tempUnit {
    color: #006dcc;
    &:hover {
      color: #005096;
    }
  }
}
</style>
