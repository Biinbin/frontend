<template>
  <div class="home">
    <h2>Villes par Pays</h2>
    <div class="form-group">
      <label for="country">Pays:</label>
      <select class="form-control" v-model="data.editedCity.country" id="selectCountry" @input="fetchCities()">
        <option value="" selected disabled hidden>Choisissez un pays</option>
        <option
          v-for="country in data.allCountries"
          :key="country.id"
          :value="country._links.cities.href"
        >
          {{ country.name }}
        </option>
      </select>
      <div class="col">
        <CityList v-bind:cities="data.allCities" 
        :tabCountry="false"/>
      </div>
    </div>
  </div>
</template>

<script setup>

import { reactive, onMounted } from "vue";
import CityList from "@/components/CityList.vue";
// @ is an alias to /src
const emptyCity = {
  id: "",
  name: "",
  population: 1,
  country: "",
};

const data = reactive({
  allCities: [],
  allCountries: [],
  editedCity: { ...emptyCity },
});

function fetchCities() {
  // Utilise l'API ad-hoc pour avoir le pays de chaque ville
  if(document.getElementById("selectCountry").value != ""){
  fetch(document.getElementById("selectCountry").value)
    .then((response) => response.json())
    .then((json) => {
      data.allCities = json._embedded.cities;
    })
    .catch((error) => alert(error));
}}

// Utilise l'API REST auto-générée pour avoir les pays
function fetchCountries() {
  fetch("api/countries")
    .then((response) => response.json())
    .then((json) => {
      data.allCountries = json._embedded.countries;
    })
    .catch((error) => alert(error));
}

// Au chargement du composant
onMounted(() => {
  fetchCities(); // On récupère les villes (pour la table)
  fetchCountries(); // On récupère les pays (pour le sélecteur de pays)
});
</script>
