<script setup>
import { ref } from 'vue'
import BorderLine from './BorderLine.vue'
import WeatherForecastDay from './WeatherForecastDay.vue'
import WeatherInfo from './WeatherInfo.vue'

defineProps({
  place: Object,
})
const emit = defineEmits(['delete-place'])
const showDetail = ref(false)
const removePlace = (placeName) => {
  emit('delete-place', placeName)
  showDetail.value = false
}
</script>

<template>
  <div
    :class="place.current.is_day === 1 ? 'bg-day' : 'bg-night'"
    class="text-white p-10 rounded-lg shadow-lg hover:shadow-xl transition-shadow duration-300 gap-6 mb-6 relative overflow-hidden"
  >
    <!-- Location & time -->
    <div class="mb-1 md:mb-2 flex flex-col sm:flex-row justify-between items-center">
      <div class="flex items-center justify-center gap-2">
        <i class="fa-solid fa-location-dot text-xl"></i>
        <h1 class="text-3xl md:text-xl font-semibold">{{ place.location.name }}</h1>
      </div>
      <div class="flex items-center justify-center gap-2">
        <i class="fa-solid fa-clock"></i>
        <h1 class="text-xl font-semibold">
          {{ String(new Date(place.location.localtime).getHours()).padStart(2, '0') }}:{{
            String(new Date(place.location.localtime).getMinutes()).padStart(2, '0')
          }}
        </h1>
      </div>
    </div>

    <!-- current weather -->
    <div class="text-center flex-1">
      <img
        :src="place.current.condition.icon"
        alt="icon"
        class="mx-auto w-24 sm:w-36 md:w-50 -mb-2 md:-mb-6"
      />
      <h1 class="text-6xl sm:text-7xl md:text-9xl mb-2 -mr-2">
        {{ Math.round(place.current.temp_c) }}&deg;
      </h1>
      <p class="text-xl md:text-2xl">{{ place.current.condition.text }}</p>
    </div>

    <BorderLine />

    <!-- forecast -->
    <div v-for="(day, idx) in place.forecast.forecastday" :key="idx">
      <!-- Weather daily forecast component goes here -->
      <WeatherForecastDay :day="day" />
    </div>

    <!-- info -->
    <Transition name="fade">
      <div v-show="showDetail">
        <!-- Weather info component goes here -->
        <WeatherInfo
          :place="place"
          @close-info="showDetail = false"
          @remove-place="removePlace(place.location.name)"
        />
      </div>
    </Transition>

    <!-- forecast btn -->
    <div class="flex justify-end items-center gap-1 mt-10 md:mt-8">
      <button @click="showDetail = true">
        More <i class="fa-solid fa-arrow-right text-base md:text-sm -mb-px"></i>
      </button>
    </div>
  </div>
</template>

<style scoped>
.bg-day {
  background-color: #8ec5fc;
  background-image: linear-gradient(62deg, #8ec5fc 0%, #e0c3fc 100%);
}
.bg-night {
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
