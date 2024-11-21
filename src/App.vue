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
  </main>
</template>

<script setup>
import { ref } from 'vue';
import SearchInput from './components/SearchInput.vue'

const places = ref([])

const addPlace = (data) => {
  places.value.push(data)
}
</script>
