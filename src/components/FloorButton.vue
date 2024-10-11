<script setup lang="ts">


const props = defineProps({
  elevator: String,
  floor: Number
})
</script>

<template>
  <button class="buttonStyle">
    {{ props.floor }}
  </button>
</template>

<style scoped>
.buttonStyle {
  margin: 2px;
  cursor: pointer;
}

</style>
