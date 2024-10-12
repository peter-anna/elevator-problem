<script setup lang="ts">

const props = defineProps({
  name: String,
  currentFloor: Number,
  status: String,
})

</script>

<template>
  <div class="content">
    <div>Lift {{ props.name }}</div>
    <div>Destination:
      <button class="floorButton" @click="$emit('destinationSelected',props.name!, 0, props.currentFloor!)">0</button>
      <button class="floorButton" @click="$emit('destinationSelected',props.name!, 1, props.currentFloor!)">1</button>
      <button class="floorButton" @click="$emit('destinationSelected',props.name!, 2, props.currentFloor!)">2</button>
      <button class="floorButton" @click="$emit('destinationSelected',props.name!, 3, props.currentFloor!)">3</button>
      <button class="floorButton" @click="$emit('destinationSelected',props.name!, 4, props.currentFloor!)">4</button>
      <button class="floorButton" @click="$emit('destinationSelected',props.name!, 5, props.currentFloor!)">5</button>
      <button class="floorButton" @click="$emit('destinationSelected',props.name!, 6, props.currentFloor!)">6</button>
    </div>
    <div>Status: {{ props.status }}</div>
    <div>Current floor: {{ props.currentFloor }}</div>
  </div>
</template>

<style scoped>
.content {
  width: 50%;
  padding: 1rem;
  display: flex;
  flex-direction: column;
  border: 1px solid lightgray;
  border-radius: 5px;
}
.floorButton {
  margin: 2px;
  cursor: pointer;
}
</style>
