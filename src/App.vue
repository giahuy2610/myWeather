<template>
  <v-app id="app">
    <v-card
      width="fit-content"
      id="tool-bar"
      class="py-2 rounded-pill d-flex flex-column"
    >
      <v-btn
        class="mx-2 mb-1"
        fab
        small
        @click="$vuetify.theme.dark = !$vuetify.theme.dark"
      >
        <v-icon dark>
          {{
            $vuetify.theme.dark
              ? "mdi-weather-night"
              : "mdi-white-balance-sunny"
          }}</v-icon
        >
      </v-btn>
      <v-btn class="mx-2 mt-1" fab small>
        <v-icon dark> mdi-translate </v-icon>
      </v-btn>
    </v-card>

    <div class="ma-0 pa-0 d-flex justify-center align-center">
      <v-card
        class="px-5 py-8 ma-5 rounded-xl d-flex flex-column align-center justify-space-between"
        style="border: 5px solid white"
        width="100%"
        height="100vh"
        max-width="350px"
        max-height="600px"
        color="primary"
        :img="$vuetify.theme.dark ? imgs[1] : imgs[3]"
      >
        <div style="width: 100%" class="d-flex flex-column align-center">
          <v-text-field
            class="rounded-xl"
            outlined
            label="Search"
            prepend-inner-icon="mdi-map-marker"
            v-model="cityTemp"
            @keyup.enter="changeCity"
            style="width: 100%; max-height: 75px"
          ></v-text-field>
          <h3>{{ weather.name }}, {{ weather.sys.country }}</h3>
          <p>{{ date }}, {{ time }}</p>

          <v-card
            class="pa-5 rounded-xl d-flex justify-center align-center"
            id="temperature-card"
            width="240px"
            height="80px"
            >{{ temperature }}<span><sup>o</sup>C</span></v-card
          >
          <p class="text-uppercase">{{ weather.weather[0].description }}</p>
        </div>

        <div
          class="mt-5 d-flex justify-space-around"
          id="cards"
          style="width: 100%"
        >
          <v-card
            class="d-flex flex-column justify-center align-center rounded-lg card"
            width="30%"
            height="80px"
          >
            <v-icon>mdi-eye-outline</v-icon>
            <p>{{ weather.visibility }}m</p>
          </v-card>
          <v-card
            class="d-flex flex-column justify-center align-center rounded-lg card"
            width="30%"
            height="80px"
          >
            <v-icon>mdi-weather-windy</v-icon>
            <p>{{ weather.wind.speed }}m/s</p>
          </v-card>
          <v-card
            label="cloudy"
            class="d-flex flex-column justify-center align-center rounded-lg card"
            width="30%"
            height="80px"
          >
            <v-icon>mdi-weather-partly-cloudy</v-icon>
            <p>{{ weather.clouds.all }}%</p>
          </v-card>
        </div>
      </v-card>
    </div>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      city: "Ha Noi",
      wind: 3.03,
      visibility: 1000,
      sunshine: 51,
      cityTemp: "Ha Noi",
      darkMode: false,
      apikey: "d922a68d965d80be3f5d94b63ac9f993",
      weather: "",
      temperature: "",
      date: "",
      time: "",
      imgs: [
        "https://i.pinimg.com/564x/96/c3/72/96c372bbc2a4326cd4e88f34a6c48303.jpg", //winter
        "https://i.pinimg.com/564x/15/f2/3e/15f23e5b65cd5a82f7c4d35e9f74049b.jpg", //night
        "https://i.pinimg.com/564x/0c/5d/e3/0c5de3d82a087a5d877b63a899fdd06d.jpg", //morning
        "https://i.pinimg.com/564x/c3/06/e9/c306e9881df19d3d7f89f49bee45b066.jpg",
      ],
    };
  },
  methods: {
    async changeCity() {
      this.city = this.cityTemp;
      let apiURL = `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=d922a68d965d80be3f5d94b63ac9f993`;
      const res = await fetch(apiURL);
      this.weather = await res.json();
      this.temperature = parseInt(this.weather.main.temp - 273.15);
    },
  },
  mounted() {
    setInterval(() => {
      const currentDate = new Date();
      this.date = currentDate.toLocaleDateString();
      this.time = currentDate.toLocaleTimeString();
      console.log(currentDate.getHours());
    }, 1000);
    this.changeCity();
    let currentDate = new Date();
    if (currentDate.getHours() >= 16 || currentDate.getHours() <= 6) {
      console.log((this.$vuetify.theme.dark = true));
    }
  },
};
</script>

<style lang="scss">
#app {
  display: flex;
  align-items: center;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background: rgb(14, 10, 10);
}

#temperature-card {
  background: rgba(188, 162, 162, 0.05);
  box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
  backdrop-filter: blur(2.5px);
  -webkit-backdrop-filter: blur(2.5px);
  border-radius: 10px;
  border: 1px solid rgba(255, 255, 255, 0.18);
  font-size: 50px;
  font-weight: bold;
}

#cards .card {
  background: rgba(188, 162, 162, 0.05);
  box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
  backdrop-filter: blur(2.5px);
  -webkit-backdrop-filter: blur(2.5px);
  border-radius: 10px;
  border: 1px solid rgba(255, 255, 255, 0.18);
}

#cards .card p {
  margin: 0;
}

#tool-bar {
  position: fixed;
  transform: translateY(50vh);
  z-index: 99999;
  opacity: 0.6;
}
</style>
