<template>
  <div v-for="city in savedCities" :key="city.id">
    <CityCard :city="city" @click="goToCityView(city)" />
  </div>

  <p v-if="savedCities.length === 0">
    No locations added. To start tracking a location, search in the field above.
  </p>
</template>

<script setup>
import CityCard from "./CityCard.vue";
import axios from "axios";
import { ref } from "vue";
import { useRouter } from "vue-router";

const savedCities = ref([]);
const getCities = async () => {
  if (localStorage.getItem("savedCities")) {
    savedCities.value = JSON.parse(localStorage.getItem("savedCities"));

    const requests = [];
    savedCities.value.forEach((city) => {
      console.log("hello");
      try {
        requests.push(
          axios.get(
            `https://api.openweathermap.org/data/2.5/weather?lat=${city.coords.latitude}&lon=${city.coords.longitude}&appid=7efa332cf48aeb9d2d391a51027f1a71&units=metric`
          )
        );
      } catch (err) {
        console.log("error occured");
      }
    });
    const weatherData = await Promise.all(requests);

    // Resolving Flicker in HomeView
    await new Promise((res) => setTimeout(res, 750));
    weatherData.forEach((value, index) => {
      savedCities.value[index].weather = value.data;
    });
  }
};
await getCities();

const router = useRouter();
const goToCityView = (city) => {
  router.push({
    name: "cityView",
    params: { state: city.state, city: city.city },
    query: {
      id: city.id,
      latitude: city.coords.latitude,
      longitude: city.coords.longitude,
    },
  });
};
</script>
