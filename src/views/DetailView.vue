<script setup>
import BikeGraph from '@/components/BikeGraph.vue';
import axios from 'axios';
import { onMounted, ref } from 'vue';
import { useRoute, useRouter } from 'vue-router';
import { GoogleMap, Marker } from 'vue3-google-map';
import { VueSpinner } from 'vue3-spinners';

const mapsKey = import.meta.env.VITE_GOOGLE_MAPS_API_KEY;
const route = useRoute();
const router = useRouter();
const placeId = route.params.id;
const loadingPlace = ref(true);
const place = ref({});
const newCreatedAt = ref('');
let center = { lat: 0, lng: 0 };

const fetchPlace = async (id) => {
  loadingPlace.value = true;
  const res = await axios
    .get(`http://localhost/api/places/${id}`)
    .catch(() => router.push('/not-found'));
  place.value = res.data.place;
  center = {
    lat: Number(place.value.lat),
    lng: Number(place.value.lng)
  };
  const date = new Date(place.value.created_at);
  newCreatedAt.value = date.toLocaleString([], {
    year: 'numeric',
    month: '2-digit',
    day: '2-digit',
    hour: '2-digit',
    minute: '2-digit'
  });
  loadingPlace.value = false;
};

onMounted(async () => {
  fetchPlace(placeId);
});
</script>

<template>
  <div v-if="!loadingPlace" id="detail">
    <div class="home-flex">
      <RouterLink to="/" style="text-decoration: none">
        <h3 class="home">
          <span>
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 576 512">
              <path
                d="M575.8 255.5c0 18-15 32.1-32 32.1l-32 0 .7 160.2c0 2.7-.2 5.4-.5 8.1l0 16.2c0 22.1-17.9 40-40 40l-16 0c-1.1 0-2.2 0-3.3-.1c-1.4 .1-2.8 .1-4.2 .1L416 512l-24 0c-22.1 0-40-17.9-40-40l0-24 0-64c0-17.7-14.3-32-32-32l-64 0c-17.7 0-32 14.3-32 32l0 64 0 24c0 22.1-17.9 40-40 40l-24 0-31.9 0c-1.5 0-3-.1-4.5-.2c-1.2 .1-2.4 .2-3.6 .2l-16 0c-22.1 0-40-17.9-40-40l0-112c0-.9 0-1.9 .1-2.8l0-69.7-32 0c-18 0-32-14-32-32.1c0-9 3-17 10-24L266.4 8c7-7 15-8 22-8s15 2 21 7L564.8 231.5c8 7 12 15 11 24z"
              />
            </svg>
          </span>
          Home
        </h3>
      </RouterLink>
    </div>

    <div class="place-info">
      <div class="text-wrapper">
        <div class="text-box">
          <h3>Name</h3>
          <h2>{{ place.name }}</h2>
          <hr />
        </div>
        <div class="text-box">
          <h3>Available bikes</h3>
          <h2>{{ place.bike_count }}</h2>
          <hr />
        </div>
        <div class="text-box">
          <h3>Latitude</h3>
          <h2>{{ place.lat }}</h2>
          <hr />
        </div>
        <div class="text-box">
          <h3>Longtitude</h3>
          <h2>{{ place.lng }}</h2>
          <hr />
        </div>
        <div class="text-box">
          <h3>Data accurate to</h3>
          <h2>{{ newCreatedAt }}</h2>
          <hr />
        </div>
      </div>
      <a href="#scroll" class="arrow">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 512 512"
          width="100%"
          style="fill: #c61f1f"
        >
          <path
            d="M233.4 406.6c12.5 12.5 32.8 12.5 45.3 0l192-192c12.5-12.5 12.5-32.8 0-45.3s-32.8-12.5-45.3 0L256 338.7 86.6 169.4c-12.5-12.5-32.8-12.5-45.3 0s-12.5 32.8 0 45.3l192 192z"
          />
        </svg>
      </a>
    </div>
    <img src="/public/waves.png" alt="" srcset="" width="100%" />

    <div class="place-detail" id="scroll">
      <div class="map">
        <h1>Location</h1>
        <GoogleMap
          :api-key="mapsKey"
          :center="center"
          :zoom="15"
          style="width: 100%; height: 500px"
        >
          <Marker :options="{ position: center }" />
        </GoogleMap>
      </div>

      <div class="graph">
        <BikeGraph :bike-count="place.bike_count"></BikeGraph>
      </div>
    </div>
  </div>
  <div v-else>
    <div
      style="
        height: 100vh;
        width: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
      "
    >
      <VueSpinner color="#c61f1f" size="80" />
    </div>
  </div>
</template>

<style></style>
