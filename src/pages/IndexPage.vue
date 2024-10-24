<template>
  <q-page class="flex flex-top">
    <h3>Météo</h3>
    <q-card class="my-card" v-for="(code, index) in dailyCodes" :key="index">
      <img :src="codesReference[code].day.image" />
      <q-card-section>
        <div class="text-h6 flex-center">Météo du jour</div>
        <q-card-section class="q-pt-none">
          {{ codesReference[code].day.description }}
          {{ tempMax }}
        </q-card-section>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script setup>
import axios from "axios";
import { ref, onMounted } from "vue";

import codes from "src/data/wmo_code.json";

defineOptions({
  name: "IndexPage",
});

const meteoDay = ref("");
const meteoIm = ref("");
const dailyCodes = ref([]);
const codesReference = ref(codes);
const tempMax = ref("");

onMounted(async () => {
  const meteoResponse = await axios.get(
    "https://api.open-meteo.com/v1/forecast?latitude=43.61&longitude=1.25&current=weather_code&daily=weather_code,temperature_2m_max,temperature_2m_min,sunrise,sunset,uv_index_max,uv_index_clear_sky_max&models=meteofrance_seamless",
  );
  const code = meteoResponse.data.current.weather_code;
  dailyCodes.value = meteoResponse.data.daily.weather_code;
  meteoDay.value = codes[code].day.description;
  meteoIm.value = codes[code].day.image;
  console.log(meteoResponse);
  tempMax.value = meteoResponse.data.current.temperature_2m_max;
});
</script>
