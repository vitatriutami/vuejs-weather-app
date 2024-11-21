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
      <SearchInput />
    </div>
  </main>
</template>

<script setup>
import SearchInput from './components/SearchInput.vue'
</script>
