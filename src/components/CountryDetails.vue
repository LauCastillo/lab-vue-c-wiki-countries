<script setup>
import { ref, onMounted, watch } from 'vue'
import { useRoute } from 'vue-router'

const route = useRoute()
const country = ref(null)
const countries = ref([])

const fetchCountry = async () => {
  if (!countries.value.length) {
    const res = await fetch('/countries.json')
    countries.value = await res.json()
  }
  country.value = countries.value.find(
    c => c.alpha3Code === route.params.alpha3Code
  )
}

// Cargar países y buscar el país al montar
onMounted(fetchCountry)
// Volver a buscar si cambia el código en la URL
watch(() => route.params.alpha3Code, fetchCountry)
const getCountryByCode = (code) => {
  return countries.value.find(c => c.alpha3Code === code)
}

</script>

<template>
  <div v-if="country" class="country-details">
    <img
      :src="`https://flagpedia.net/data/flags/icon/72x54/${country.alpha2Code.toLowerCase()}.png`"
      :alt="country.name.common"
      style="width: 200px"
    />
     <h1>{{ country.name.common }}</h1>    
      <div class="details-grid">
      <div class="label">Capital</div>
      <div>{{ country.capital[0] }}</div>
      <div class="label">Área</div>
      <div>{{ country.area }} km²</div>
      <div class="label">Fronteras</div>
      <div>
        <ul>
          <li v-for="border in country.borders" :key="border">
            <router-link
                :to="`/list/${border}`"
                class="list-group-item list-group-item-action"
                 v-if="getCountryByCode(border)">
                 <img
                :src="`https://flagpedia.net/data/flags/icon/24x18/${getCountryByCode(border).alpha2Code.toLowerCase()}.png`"
                :alt="getCountryByCode(border).name.common"
                style="width: 24px; margin-right: 8px;"
              />
               {{ getCountryByCode(border).name.common }}
            </router-link>
           
          </li>
        </ul>
      </div>
    </div>
  </div>
  <div v-else>
    <p>Selecciona un país de la lista.</p>
  </div>
</template>

<style scoped>
a {
  text-decoration: none;
  color: rgb(0, 0, 255);
}
li {
  list-style: none;
  display: flex;
  align-items: center;
  gap: 1rem;
}
.country-details {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
}

.details-grid {
  display: grid;
  grid-template-columns: 120px 1fr;
  gap: 0.5rem 1.5rem;
  width: 100%;
  max-width: 400px;
  margin-top: 1rem;
}

.label {
  font-weight: bold;
  color: #345;
}
</style>