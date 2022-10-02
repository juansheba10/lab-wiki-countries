<template>
  <div class="col-8" v-if="countryRoute">
    <img
      :src="`https://flagcdn.com/w320/${countryRoute.alpha2Code.toLowerCase()}.png`"
      alt="country flag"
      style="width: 300px"
    />
    <h1>{{ countryRoute.name.common }}</h1>
    <table class="table">
      <thead></thead>
      <tbody>
        <tr>
          <td style="width: 30%">Capital</td>
          <td>{{ countryRoute.capital[0] }}</td>
        </tr>
        <tr>
          <td>Area</td>
          <td>{{ countryRoute.area }} km<sup>2</sup></td>
        </tr>
        <tr>
          <td>Borders</td>
          <td v-if="countryRoute.borders.length === 0">
            This Country has not borders
          </td>
          <RouterLink
            v-else
            :to="`/CountriesList/${border}`"
            v-for="(border, index) in countryRoute.borders"
            :key="index"
            class="d-flex"
          >
            {{ border }}
          </RouterLink>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { computed, onMounted, ref, watch } from "vue";
import { useRoute } from "vue-router";

//data properties
const countries = ref(null);
const countryRoute = ref(null);
const route = useRoute();
//methods/functions
const getCountriesByAlphaCode = async () => {
  const alpha3Code = route.params.alpha3Code;

  const response = await fetch(
    `https://ih-countries-api.herokuapp.com/countries/${alpha3Code}`
  );
  const finalResponse = await response.json();

  countryRoute.value = finalResponse;

  return {
    countryRoute,
  };
};

console.log(countryRoute);
onMounted(() => {
  getCountriesByAlphaCode();
});

const countryCode = computed(() => {
  return route.params.alpha3Code;
});

watch(countryCode, (newCountryCode) => {
  getCountriesByAlphaCode();
});
</script>

<style></style>
