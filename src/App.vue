<template>
  <div class="app">
    <div>Current Rating: {{ currentRating }}</div>
    <StarRating
        :step="0.5"
        @update-rating="currentRating = $event"
    />
  </div>
</template>

<script setup>
import StarRating from "@/components/StarRating.vue";
import {ref} from "vue";
const currentRating = ref(0)
</script>

<style scoped>
.app {
  display: flex;
  flex-direction: column;
  gap: 15px;
  justify-content: center;
  align-items: center;
  height: 100vh;
  font-size: 25px;
  line-height: 30px;
  font-family: monospace;
  background-color: #f7f7f7;
}
</style>
