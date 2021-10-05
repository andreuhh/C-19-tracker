<template>
  <main v-if="!loading">
    <DataTitle :text="title" :dataDate="dataDate" />

    <h1>min 50</h1>
  </main>

  <main class="flex flex-col align-center justify-center" v-else>
    <div class="text-gray-500 text-3xl mt-10 mb-6">Fetching Data</div>
    <img :src="loadingImage" class="w-24 m-auto" alt="" />
  </main>
</template>

<script>
import DataTitle from "@/components/DataTitle";

export default {
  name: "Home",
  components: { DataTitle },
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
  methods: {
    async fetchCovidData() {
      const res = await fetch("https://api.covid19api.com/summary");
      const data = await res.json();
      return data;
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
