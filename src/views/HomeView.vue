<script setup>
import axios from 'axios';
import { onMounted, ref, watch } from 'vue';
import { VueSpinner } from 'vue3-spinners';

const search = ref('');
const sort = ref('desc');
const places = ref([]);
const loadingPlaces = ref(true);
let timeout = null;

watch(search, (newSearch) => {
  loadingPlaces.value = true;
  clearTimeout(timeout);

  timeout = setTimeout(() => {
    fetchPlaces(newSearch, sort.value);
  }, 1000);
});

watch(sort, () => {
  loadingPlaces.value = true;
  fetchPlaces(search.value, sort.value);
});

const fetchPlaces = async (query = '', sortQuery = 'desc') => {
  loadingPlaces.value = true;
  const res = await axios.get(`http://localhost/api/places?name=${query}&sort=${sortQuery}`);
  places.value = res.data.places;
  loadingPlaces.value = false;
};

onMounted(async () => {
  fetchPlaces();
});
</script>

<template>
  <main>
    <section id="hero">
      <div class="hero-wrap">
        <h1 class="roboto-bold text-red"><span>👻</span> Creepy Bikes</h1>
        <div class="wrap">
          <div class="search">
            <input
              type="text"
              class="searchTerm"
              placeholder="Search for bike stations..."
              v-model="search"
            />
            <button class="searchButton">
              <svg xmlns="http://www.w3.org/2000/svg" style="fill: white" viewBox="0 0 512 512">
                <path
                  d="M416 208c0 45.9-14.9 88.3-40 122.7L502.6 457.4c12.5 12.5 12.5 32.8 0 45.3s-32.8 12.5-45.3 0L330.7 376c-34.4 25.2-76.8 40-122.7 40C93.1 416 0 322.9 0 208S93.1 0 208 0S416 93.1 416 208zM208 352a144 144 0 1 0 0-288 144 144 0 1 0 0 288z"
                />
              </svg>
            </button>
          </div>
          <select name="sort" id="sort" v-model="sort">
            <option value="desc" selected>Descending</option>
            <option value="asc">Ascending</option>
          </select>
        </div>
      </div>
    </section>
    <img src="/public/waves.png" alt="" srcset="" width="100%" />

    <section id="places">
      <div v-if="!loadingPlaces" class="places-flexbox">
        <div v-for="place in places" :key="place.id" style="width: 100%">
          <RouterLink :to="`/places/${place.id}`" style="text-decoration: none">
            <div class="place-box">
              <h1>{{ place.name }}</h1>
              <h1>{{ place.bike_count }} available bikes</h1>
            </div>
          </RouterLink>
        </div>
      </div>

      <div class="loading" v-else>
        <VueSpinner color="#c61f1f" size="80" />
      </div>
    </section>
  </main>
</template>
