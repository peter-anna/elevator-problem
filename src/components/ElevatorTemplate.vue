<script setup lang="ts">
import { ELEVATOR_STATUS } from '@/constants/elevatorStatus';

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
      <button class="floorButton" @click="$emit('destinationSelected',props.name!, 0)" :disabled="props.status === ELEVATOR_STATUS.CALLED">0</button>
      <button class="floorButton" @click="$emit('destinationSelected',props.name!, 1)" :disabled="props.status === ELEVATOR_STATUS.CALLED">1</button>
      <button class="floorButton" @click="$emit('destinationSelected',props.name!, 2)" :disabled="props.status === ELEVATOR_STATUS.CALLED">2</button>
      <button class="floorButton" @click="$emit('destinationSelected',props.name!, 3)" :disabled="props.status === ELEVATOR_STATUS.CALLED">3</button>
      <button class="floorButton" @click="$emit('destinationSelected',props.name!, 4)" :disabled="props.status === ELEVATOR_STATUS.CALLED">4</button>
      <button class="floorButton" @click="$emit('destinationSelected',props.name!, 5)" :disabled="props.status === ELEVATOR_STATUS.CALLED">5</button>
      <button class="floorButton" @click="$emit('destinationSelected',props.name!, 6)" :disabled="props.status === ELEVATOR_STATUS.CALLED">6</button>
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
