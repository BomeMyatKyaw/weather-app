<script setup>
import { ref } from 'vue';
import BorderLine from './BorderLine.vue';
import WeatherForcastDay from './WeatherForcastDay.vue';
import WeatherInfo from './WeatherInfo.vue';

defineProps({
  place: Object
})

const emit = defineEmits(['delete-place'])

const showDetail = ref(false)

const removePlace = (placeName) => {
  emit('deletePlace', placeName)
  showDetail.value = false
}

</script>

<template>
  <div :class="place.current.is_day === 1 ? 'bg-day' : 'bg-night'" class="relative gap-6 p-10 mb-6 overflow-hidden text-white bg-blue-500 rounded-lg shadow-lg">
    <!-- Location & time -->
    <div class="flex items-center justify-between mb-2">
      <div class="flex items-center justify-center gap-2">
        <i class="fa-solid fa-location-dot"></i>
        <h1 class="text-3xl">{{ place.location.name }}</h1>
      </div>
      <div class="flex items-center justify-center gap-2">
        <i class="fa-solid fa-clock"></i>
        <h1 class="text-3xl">{{ new Date(place.location.localtime).getHours() }}:{{ new Date(place.location.localtime).getMinutes() }}</h1>
      </div>
    </div>

    <!-- current weather -->
    <div class="flex-1 text-center">
      <img :src="place.current.condition.icon" alt="icon" width="200" class="mx-auto -mb-10" />
      <h1 class="mb-2 text-9xl">{{ Math.round(place.current.temp_c) }}&deg;</h1>
      <p class="text-2xl">{{  place.current.condition.text }}</p>
    </div>

    <BorderLine />

    <!-- forecast -->
    <div v-for="day in place.forecast.forecastday" :key="day">
      <WeatherForcastDay :day="day" />
    </div>

    <!-- info -->
    <transition name="fade">
      <div v-show="showDetail">
        <WeatherInfo :place="place" @click="showDetail = false" @remove-place="removePlace(place.location.name)" />
      </div>
    </transition>

    <!-- forecast btn -->
    <div class="flex items-center justify-end gap-1 mt-10">
      <button @click="showDetail = true">More <i class="-mb-px text-sm fa-solid fa-arrow-right"></i></button>
    </div>
  </div>
</template>

<style scoped>
.bg-day{
  background-color: #8ec5fc;
  background-image: linear-gradient(62deg, #8ec5fc 0%, #e0c3fc 100%);
}

.bg-night{
  background-color: #07223d;
  background-image: linear-gradient(62deg, #0a2a4a 0%, #270845 100%);
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.2s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>