<script setup lang="ts">
import FloorButton from './FloorButton.vue';



const props = defineProps({
  name: String,
})
</script>

<template>
  <div class="content">
    <div>Lift {{ props.name }}</div>
    <div>Destination:
      <FloorButton :floor="0" :elevator="props.name" />
      <FloorButton :floor="1" :elevator="props.name" />
      <FloorButton :floor="2" :elevator="props.name" />
      <FloorButton :floor="3" :elevator="props.name" />
      <FloorButton :floor="4" :elevator="props.name" />
      <FloorButton :floor="5" :elevator="props.name" />
      <FloorButton :floor="6" :elevator="props.name" />
    </div>
    <div>Status:</div>
    <div>Current floor:</div>
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
}
</style>
