<template>
  <main v-if="!loading">
    <DataTitle :text="title" :dataDate="dataDate" />
    <DataBoxes :stats="stats" />
    <CountrySelect @get-country="getCountryData" :countries="countries" />

    <button
      v-if="stats.Country"
      class="
        bg-green-700
        text-white
        rounded
        p-3
        mt-10
        focus:outline-none
        hover:bg-green-600
      "
      @click="clearCountryData"
    >
      Clear country
    </button>
  </main>

  <main class="flex flex-col align-center justify-center" v-else>
    <div class="text-gray-500 text-3xl mt-10 mb-6">Fetching Data</div>
    <img :src="loadingImage" class="w-24 m-auto" alt="" />
  </main>

  <div v-if="weatherData.weather">
    <h1>{{ Math.round(weatherData.weather.main.temp) }}&deg;</h1>
    <h2>{{ weatherData.weather.main.humidity }}</h2>
    <h2>{{ weatherData.weather.main.pressure }}</h2>
  </div>

  <input type="text" v-model="weatherData.city" @keyup.enter="getWeather" />
</template>

<script>
import DataTitle from "@/components/DataTitle";
import DataBoxes from "@/components/DataBoxes";
import CountrySelect from "@/components/CountrySelect";
import { reactive } from "vue";
import axios from "axios";

export default {
  name: "Home",
  components: { DataTitle, DataBoxes, CountrySelect },
  data() {
    return {
      loading: true,
      title: "Global",
      dataDate: "",
      stats: {},
      countries: [],
      loadingImage: require("../assets/hourglass.gif"),
    };
  },

  setup() {
    let weatherData = reactive({
      city: "",
      weather: null,
    });

    const apiUrl = `http://localhost:3000/`;

    const getWeather = () => {
      axios(`${apiUrl}?q=${weatherData.city}`).then((response) => {
        weatherData.weather = response.data;
        console.log(weatherData.weather);
      });
    };

    return {
      weatherData,
      getWeather,
    };
  },

  methods: {
    async fetchCovidData() {
      const res = await fetch("https://api.covid19api.com/summary");
      const data = await res.json();
      return data;
    },
    getCountryData(country) {
      this.stats = country;
      this.title = country.Country;
    },
    async clearCountryData() {
      this.loading = true;
      const data = await this.fetchCovidData();
      this.title = "Global";
      this.stats = data.Global;
      this.loading = false;
    },
  },
  async created() {
    const data = await this.fetchCovidData();

    this.dataDate = data.Date;
    this.countries = data.Countries;
    this.stats = data.Global;
    this.loading = false;
  },
};
</script>
