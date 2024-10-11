<script setup lang="ts">
import ElevatorTemplate from './components/ElevatorTemplate.vue';
import Floor from './components/FloorTemplate.vue'
</script>

<template>
  <div class="content">
    <Floor :level="6" />
    <Floor :level="5" />
    <Floor :level="4" />
    <Floor :level="3" />
    <Floor :level="2" />
    <Floor :level="1" />
    <Floor :level="0" />
  </div>
  <div class="lifts">
    <ElevatorTemplate name="A" />
    <ElevatorTemplate name="B" />
  </div>
</template>

<style scoped>
.content {
  display: flex;
  flex-direction: column;
  gap: 5px;
}
.lifts {
  display: flex;
  flex-direction: row;
  gap: 3rem;
  padding-top: 1rem;
}
</style>
