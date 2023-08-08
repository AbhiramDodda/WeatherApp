<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input
        v-model="searchQuery"
        @input="getSearchResults"
        type="text"
        placeholder="Search for a city or state"
        class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]"
      />
      <ul
        class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]"
        v-if="searchResults"
      >
        <p v-if="searchError">Sorry, Something went wrong</p>
        <p v-if="!searchError && searchResults.length === 0">
          No results found
        </p>
        <template v-else>
          <li
            v-for="searchResult in searchResults"
            :key="searchResult.id"
            class="py-2 cursor-pointer"
            @click="viewCity(searchResult)"
          >
            {{ searchResult.place_name }}
          </li>
        </template>
      </ul>
    </div>
    <div class="flex flex-col gap-4">
      <Suspense>
        <SavedCities />
        <template #fallback> <CityCardLayout /> </template>
      </Suspense>
    </div>
  </main>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";
import { useRouter } from "vue-router";
import SavedCities from "../components/SavedCities.vue";
import CityCardLayout from "../components/CityCardLayout.vue";

const router = useRouter();

const viewCity = (searchResult) => {
  //console.log(searchResult);
  const [city, state] = searchResult.place_name.split(",");
  // console.log(city, state);
  router.push({
    name: "cityView",
    params: { state: state.replaceAll(" ", ""), city: city },
    query: {
      latitude: searchResult.geometry.coordinates[1],
      longitude: searchResult.geometry.coordinates[0],
      preview: true,
    },
  });
};

const mapboxAPIKey =
  "pk.eyJ1Ijoiam9obmtvbWFybmlja2kiLCJhIjoiY2t5NjFzODZvMHJkaDJ1bWx6OGVieGxreSJ9.IpojdT3U3NENknF6_WhR2Q";
const searchQuery = ref("");
const queryTimeout = ref(null);
const searchResults = ref(null);
const searchError = ref(null);

const getSearchResults = () => {
  clearTimeout(queryTimeout.value);
  queryTimeout.value = setTimeout(async () => {
    if (searchQuery.value !== "") {
      try {
        const result = await axios.get(
          `https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapboxAPIKey}&types=place`
        );
        searchResults.value = result.data.features;
      } catch {
        searchError.value = true;
      }

      return;
    }
    searchResults.value = null;
  }, 300);
};
</script>
