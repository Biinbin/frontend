<template>
  <div>
    <table class="table table-bordered table-sm table-hover">
      <thead>
        <tr>
          <th>Nom</th>
          <th>Population</th>
          <th v-if="tabCountry">Pays</th>          
        </tr>
      </thead>
      <tbody>
        <tr v-for="city in cities" :key="city.id">
          <td>{{ city.name }}</td>
          <td>{{ city.population }}</td>
          <td v-if="tabCountry">{{ city.country.name }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
const props = defineProps({
  cities: {
    type: Array,
    required: true,
  },
  tabCountry: {
    type: Boolean,
    required: true,
  }
});
</script>
