<script setup lang="ts">


const props = defineProps({
  level: Number,
})
</script>

<template>
  <div class="floor">
    <div>{{ props.level }}.</div>
    <div><img src="../assets/arrowUp.svg" alt="Arrow Up" class="arrow"/></div>
    <div><img src="../assets/arrowDown.svg" alt="Arrow Down" class="arrow"/></div>
  </div>
</template>

<style scoped>
.floor {
  width: 100vh;
  height: 3rem;
  border-style: solid;
  border-width: 1px;
  border-color: lightgray;
  border-radius: 5px;
  line-height: 3rem;
  padding-left: 5px;
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 1rem;
}
.arrow {
  height: 1.5rem;
  align-self: center;
  display: block;
  cursor: pointer;
}
</style>
