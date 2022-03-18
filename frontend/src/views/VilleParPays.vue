<template>
  <div class="home">
    <h2>Villes par pays</h2>
        <label for="country">Choisir un pays : </label>
        <select @change="onChange($event)" class="form-control" v-model="data.editedCity.country">
            <option disabled value="0">Choisissez un pays</option>
            <option
                v-for="country in data.allCountries"
                :key="country.id"
                :value="country._links.self.href"
              >
                {{ country.name }}
            </option>
        </select>
    <hr>
    <city-list />
  </div>
</template>

<script setup>
import { reactive, onMounted } from "vue";
// @ is an alias to /src
import CityList from "@/components/CityList.vue";

function onChange($event) {
     console.log(event.target.value)
}

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
  fetch("api/allCities")
    .then((response) => response.json())
    .then((json) => {
      data.allCities = json;
    })
    .catch((error) => alert(error));
}

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