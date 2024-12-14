<template>
  <main class="z-10 first-letter:px-2 sm:px-4 lg:px-8 py-4 md:py-8">
    <!-- Date -->
    <div class="text-center mb-8 text-lg sm:text-xl lg:text-2xl font-semibold">
      {{
        new Date().toLocaleDateString('en-us', {
          weekdat: 'long',
          year: 'numeric',
          month: 'long',
          day: 'numeric',
        })
      }}
    </div>
    <!-- Search -->
    <div class="mb-6">
      <SearchInput @place-data="addPlace" />
    </div>
    <!-- Weather cards -->
    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
      <div v-for="(place, idx) in places" :key="idx">
        <WeatherCard :place="place" @delete-place="deletePlace" />
      </div>
    </div>
  </main>
</template>

<script setup>
import { ref } from 'vue'
import SearchInput from './components/SearchInput.vue'
import WeatherCard from './components/WeatherCard.vue'

const places = ref([])

const addPlace = (data) => {
  places.value.push(data)
}

const deletePlace = (name) => {
  if (confirm('Are you sure?')) {
    places.value = places.value.filter((p) => p.location.name !== name)
  }
}
</script>
