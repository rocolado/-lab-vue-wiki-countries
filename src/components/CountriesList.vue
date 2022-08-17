<template>
    <div class="container">
      <h1 class="text-center my-3">Countries List</h1>

      <div v-if="this.countries" class="row">
        <div class="col-4">
          <ul class="list-group">
            <router-link v-bind:to="`/list/${country.alpha3Code}`" v-for="(country, index) in countries" :key="index">
              <li class="list-group-item | d-flex flex-column justify-content-center">
                <img v-bind:src="`https://flagpedia.net/data/flags/icon/72x54/${country.alpha2Code.toLowerCase()}.png`" alt="">

                <p class="text-center fw-bold">{{country.name.common}}</p>
                <p class="text-center fw-bold">{{country.alpha2Code}}</p>
              </li>
            </router-link>
          </ul>
        </div>
        
        <div class="col-8">
          <router-view></router-view>
        </div>
      </div>

      <div v-else class="row">
        <Spinner/>
      </div>
    </div>
</template>

<script>
  import Spinner from "../components/Spinner.vue";
  // import CountryDetails from "./CountryDetails.vue";

  export default {
    name: "CountriesList",
    components: {
      Spinner,
      // CountryDetails,
    },

    data() {
      return {
        countries: null,
      };
    },

    methods: {
      async fetchCountries() {
        const response = await fetch("https://ih-countries-api.herokuapp.com/countries");

        const finalResponse = await response.json();

        this.countries = finalResponse.sort((a, b) => {
          return a.name.common.localeCompare(b.name.common);
        });
      },
    },

    created() {
      this.fetchCountries();
    }
}
</script>

<style>

</style>