<template>
  <!-- wrapper div de bootstrap -->
  <div class="container">
    <h1 class="text-center my-3">Country List</h1>
    <div v-if="this.countries" class="row">
      <div class="col-5">
        <div class="list-group">
          <router-link
            v-for="(country, index) in countries"
            :key="index"
            :to="`/list/${country.alpha3Code}`"
            class="list-group-item d-flex flex-column justify-content-center"
          >
            <img
              class="flag"
              :src="`https://flagpedia.net/data/flags/icon/72x54/${country.alpha2Code.toLowerCase()}.png`"
              alt=""
            />
            <p class="text-center fw-bold">
              {{ country.name.common }}
            </p>
          </router-link>
        </div>
      </div>
      <div class="col-7">
        <router-view />
      </div>
    </div>
    <div v-else class="row">
      <Spinner text="Loading Countries..." />
    </div>
  </div>
</template>

<script>
import Spinner from "../components/Spiner.vue";
export default {
  name: "CountriesList",
  components: { Spinner },
  data() {
    return {
      //definimos un valor de datos estilo array para recibir la info del api
      countries: null,
    };
  },
  methods: {
    async fetchCountries() {
      const response = await fetch(
        "https://ih-countries-api.herokuapp.com/countries"
      );
      const finalResponse = await response.json();
      console.log(finalResponse);
      // console.log(finalResponse);//VERIFICAMOS MEDIANTES UNA LLAMADA A CONSOLA QUE RECIBIMOS LOS DATOS
      this.countries = finalResponse.sort((a, b) => {
        return a.name.common.localeCompare(b.name.common);
      });
    },
  },
  //usamos el created hook para hacer nuestra llamada inicial a nuestra base de datos.
  //no usamos async en este caso porque la asincronia la maneja la funcion fetchCountries. El hook created() solo se encarga de llamar la funcion fetchCountries
  created() {
    this.fetchCountries();
  },
};
</script>

<style>
.flag {
  width: 100px;
  height: 60px;
}
</style>
