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
    class="text-white p-6 sm:p-8 md:p-10 rounded-lg shadow-lg gap-6 mb-6 hover:shadow-xl transition-shadow duration-300 relative overflow-hidden"
  >
    <!-- Location & time -->
    <div class="mb-4 flex flex-col sm:flex-row justify-between items-center gap-4">
      <div class="flex items-center gap-2">
        <i class="fa-solid fa-location-dot"></i>
        <h1 class="text-2xl sm:text-3xl">{{ place.location.name }}</h1>
      </div>
      <div class="flex items-center gap-2">
        <i class="fa-solid fa-clock"></i>
        <h1 class="text-2xl sm:text-3xl">
          {{ new Date(place.location.localtime).getHours() }}:{{
            new Date(place.location.localtime).getMinutes()
          }}
        </h1>
      </div>
    </div>

    <!-- Current weather -->
    <div class="text-center flex-1 mb-4">
      <img :src="place.current.condition.icon" alt="icon" class="mx-auto w-24 sm:w-32 md:w-40 -mb-8" />
      <h1 class="text-6xl sm:text-7xl md:text-9xl mb-2 -mr-2 sm:-mr-4">
        {{ Math.round(place.current.temp_c) }}&deg;
      </h1>
      <p class="text-xl sm:text-2xl">{{ place.current.condition.text }}</p>
    </div>

    <BorderLine />

    <!-- Forecast -->
    <div
      v-for="(day, idx) in place.forecast.forecastday"
      :key="idx"
      class="mt-4 grid grid-cols-1 sm:grid-cols-2 gap-4"
    >
      <WeatherForecastDay :day="day" />
    </div>

    <!-- Info -->
    <Transition name="fade">
      <div v-show="showDetail">
        <WeatherInfo
          :place="place"
          @close-info="showDetail = false"
          @remove-place="removePlace(place.location.name)"
        />
      </div>
    </Transition>

    <!-- Forecast button -->
    <div class="flex justify-end items-center gap-1 mt-8 sm:mt-10">
      <button @click="showDetail = true" class="text-sm sm:text-base">
        More <i class="fa-solid fa-arrow-right text-sm -mb-px"></i>
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

