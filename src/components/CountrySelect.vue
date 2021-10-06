<template>
  <select
    @change="onChangeCountry"
    v-model="selected"
    class="form-select mt-5 block w-full border p-3 rounded"
  >
    <option value="0">Select Country</option>
    <option v-for="country in countries" :key="country.ID" :value="country.ID">
      {{ country.Country }}
    </option>
  </select>
</template>

<script>
export default {
  name: "CountrySelect",
  props: ["countries"],
  data() {
    return {
      selected: 0,
    };
  },
  methods: {
    onChangeCountry() {
      //fins selected country
      const country = this.countries.find((item) => item.ID === this.selected);

      console.log(country);

      this.$emit("get-country", country);
    },
  },
};
</script>

<style>
</style>