<template>
  <main>
    <!-- Date -->
    <div class="text-center mb-8">
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
    <div>
      <SearchInput @place-data="addPlace" />
    </div>
    <!-- Weather cards -->
    <div>
      <p v-for="place in places">{{ place.location.name }}</p>
    </div>
  </main>
</template>

<script setup>
import { ref } from 'vue'
import SearchInput from './components/SearchInput.vue'

const places = ref([])

const addPlace = (data) => {
  places.value.push(data)
}
</script>
