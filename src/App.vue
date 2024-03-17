<template>
  <div class="app">
    <div>Current Rating: {{ currentRating }}</div>
    <StarRating
        :step="0.1"
        :rating="currentRating"
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
/* this block can be deleted  */
.app {
  display: flex;
  flex-direction: column;
  gap: 15px;
  justify-content: center;
  align-items: center;
  height: 100vh;
  font-size: 25px;
  line-height: 30px;
  font-family: 'JetBrains Mono', monospace;
  background-color: #f7f7f7;
}
/* this block can be deleted  */
</style>
