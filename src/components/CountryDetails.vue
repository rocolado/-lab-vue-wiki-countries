<template>
  <!-- Using individual data properties -->
  <div>
    <img class="mt-5" v-bind:src="`https://flagcdn.com/w320/${alpha2Code.toLowerCase()}.png`" alt="">
    
    <h1>{{name}}</h1>

    <ul class="list-group list-group-flush">
      <li class="list-group-item | d-flex justify-content-between">
        <p class="fw-bold">Capital</p>
        <p>{{capital}}</p>
      </li>

      <li class="list-group-item | d-flex justify-content-between">
        <p class="fw-bold">Area</p>
        <p>{{area}} km2</p>
      </li>

      <li class="list-group-item | d-flex justify-content-between">
        <p class="fw-bold">Borders</p>

        <p v-if="borders.length === 0">This country has no borders</p>

        <div v-else v-for="(border, index) in borders" :key="index">
          <router-link v-bind:to="`/list/${border}`">{{border}}</router-link>
        </div>
      </li>
    </ul>
  </div>

  <!-- Using object to store data properties -->
  <!-- <div>
    <img class="mt-5" :src="`https://flagcdn.com/w320/${alpha2Code.toLowerCase()}.png`" alt="">
    
    <h1>{{countryInfo.name.common}}</h1>

    <ul class="list-group list-group-flush">
      <li class="list-group-item | d-flex justify-content-between">
        <p class="fw-bold">Capital</p>
        <p>{{countryInfo.capital[0]}}</p>
      </li>

      <li class="list-group-item | d-flex justify-content-between">
        <p class="fw-bold">Area</p>
        <p>{{countryInfo.area}} km2</p>
      </li>

      <li class="list-group-item | d-flex justify-content-between">
        <p class="fw-bold">Borders</p>

        <p v-if="countryInfo.borders.length === 0">This country has no borders</p>

        <div v-else v-for="(border, index) in countryInfo.borders" :key="index">
          <router-link :to="`/list/${border}`">{{border}}</router-link>
        </div>
      </li>
    </ul>
  </div> -->
</template>

<script>
  export default {
    name: "CountryDetails",

    data() {
      return {
        // Individual data properties
        name: "",
        capital: "",
        area: "",
        borders: [],
        alpha2Code: "",
        alpha3Code: "",

        // Data API on an object
        // countryInfo: {

        // },
      };
    },

    methods: {
      async getCountryByAlphaCode() {
        this.alpha3Code = this.$route.params.alpha3Code;
        console.log(this.alpha3Code)

        const response = await fetch(`https://ih-countries-api.herokuapp.com/countries/${this.alpha3Code}`);

        const finalResponse = await response.json();

        // Individual data properties
        this.name = finalResponse.name.common;
        this.capital = finalResponse.capital[0];
        this.area = finalResponse.area;
        this.borders = finalResponse.borders;
        this.alpha2Code = finalResponse.alpha2Code;

        // Data API on an object
        // this.countryInfo = finalResponse;
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
    }
  };
</script>

<style>

</style>