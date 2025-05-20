<script setup>
import { ref, onMounted } from 'vue'

const countries = ref([])

onMounted(async () => {
  const res = await fetch('/countries.json')
  countries.value = await res.json()
})
</script>
<template>
    <div class="row">

  <div class="countries-list" style="max-height: 90vh; overflow: scroll">
    <ul class="list-group">
      <router-link
        v-for="country in countries"
        :key="country.alpha3Code"
        class="list-group-item list-group-item-action"
        :to="`/list/${country.alpha3Code}`"
      >
     <li> <img
      :src="`https://flagpedia.net/data/flags/icon/72x54/${country.alpha2Code.toLowerCase()}.png`"
      :alt="country.name.common"
      style="width: 50px"
    />
       <span>{{ country.name.common }}</span> 
        </li>
      </router-link>
    </ul>
  </div>
<div class="right-column" style="max-height: 90vh; overflow: scroll">
      <router-view />
    </div>

      </div>
</template>


<style scoped>
a{
  text-decoration: none;
  color: black;
}
a.router-link-active {
  background-color: #6486ab;
  color: white;
}
li {
  list-style: none;
  display:flex;
  align-items: center;
  gap: 1rem;
  flex-direction: column;
}
.row {
  display: flex;
  flex-direction: row;
  gap: 1rem;
}
.right-column {
  flex: 3;

}
.countries-list {
  display: flex;
  flex-direction: column;
  max-height: 90vh;
  overflow: scroll;
}
.list-group {
  display: flex;
  flex-direction: column;
  gap: 0.9rem;
}
</style>