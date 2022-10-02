<template>
  <div class="container">
    <!-- Bootstrap row wrapper div -->
    <div class="row" v-if="countries">
      <h1>Countries List</h1>
      <!-- Countries List (Bootstrap column) -->
      <div class="col-5" style="max-height: 90vh; overflow: scroll">
        <div class="list-group">
          <router-link
            class="list-group-item list-group-item-action"
            v-for="(country, index) in countries"
            :key="index"
            :to="`/CountriesList/${country.alpha3Code}`"
          >
            <img
              class="list-group-item country-flag"
              :src="`https://flagpedia.net/data/flags/icon/72x54/${country.alpha2Code.toLowerCase()}.png`"
              alt=""
            />
            {{ country.name.common }}
          </router-link>
        </div>
      </div>

      <div class="col-7"><RouterView /></div>
    </div>
    <div v-else class="col-12"><Spinner text="Loading.." /></div>
  </div>
</template>

<script setup>
//data properties
import CountryDetails from "./CountryDetails.vue";
import { ref } from "vue";
import Spinner from "./Spinner.vue";

const countries = ref(null);

//methods/functions
const fetchCountries = async () => {
  const response = await fetch(
    "https://ih-countries-api.herokuapp.com/countries"
  );

  const finalResponse = await response.json();

  const sortedCountries = finalResponse.sort((a, b) => {
    return a.name.common.localeCompare(b.name.common);
  });

  countries.value = sortedCountries;
};

fetchCountries();
</script>

<style scoped>
.country-flag {
  width: 150px;
}
</style>
