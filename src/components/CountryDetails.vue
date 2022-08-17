<template>
  <div>
    <img
      class="flag"
      v-bind:src="`https://flagpedia.net/data/flags/icon/72x54/${alpha2Code.toLowerCase()}.png`"
      alt=""
    />
    <h1>{{ name }}</h1>
    <ul class="list-group list-group-flush">
      <li
        class="list-group-item d-flex justify-content-between align-items-center"
      >
        <p class="fw-bold">Capital:</p>
        <p class="me-5">{{ capital[0] }}</p>
      </li>

      <li
        class="list-group-item d-flex justify-content-between align-items-center"
      >
        <p class="fw-bold">Area</p>
        <p class="me-5">{{ area }} km2</p>
      </li>

      <li class="list-group-item">
        <p class="fw-bold">Borders:</p>

        <p v-if="borders.length === 0">This country has no borders</p>

        <p v-else v-for="(border, index) in borders" :key="index">
          <router-link :to="`/list/${border}`">{{ border }}</router-link>
        </p>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: "",
      capital: "",
      alphacode: "",
      area: "",
      borders: [],
      alpha2Code: "",
    };
  },
  methods: {
    async getCountryByAlphaCode() {
      this.alphacode = this.$route.params.alpha3Code;
      const response = await fetch(
        `https://ih-countries-api.herokuapp.com/countries/${this.alphacode}`
      );
      const finalResponse = await response.json();
      console.log(finalResponse);
      this.name = finalResponse.name.common;
      this.capital = finalResponse.capital[0];
      this.area = finalResponse.area;
      this.borders = finalResponse.borders;
      this.alpha2Code = finalResponse.alpha2Code;
    },
  },
  mounted() {
    this.getCountryByAlphaCode();
  },
  computed: {
    countryCode() {
      return this.$route.params.alpha3Code;
    },
  },
  watch: {
    countryCode(newCountryCode) {
      this.getCountryByAlphaCode();
    },
  },
};
</script>

<style></style>
