<script setup lang="ts">
import { computed } from 'vue';

const props = defineProps({
  floor: {
    type: Number,
    required: true
  },
  elevatorA: {
    type: Object,
    required: true
  },
  elevatorB: {
    type: Object,
    required: true
  },
})

const elevatorADirection = computed(() => {
  if (props.elevatorA.currentFloor === props.floor) {
    return 'here it is';
  }
  
  if (props.elevatorA.status === 'MOVING') {
    if (props.elevatorA.currentFloor < props.floor) {
      return 'up';
    } else if (props.elevatorA.currentFloor > props.floor) {
      return 'down';
    }
  }

  return 'steady';
});

const elevatorBDirection = computed(() => {
  if (props.elevatorB.currentFloor === props.floor) {
    return 'here it is';
  }

  if (props.elevatorB.status === 'MOVING') {
    if (props.elevatorB.currentFloor < props.floor) {
      return 'up';
    } else if (props.elevatorB.currentFloor > props.floor) {
      return 'down';
    }
  }

  return 'steady';
});

</script>

<template>
  <div class="floor">
    <div>{{ props.floor }}.</div>
    <div class="liftCallers">
    <div><img src="../assets/arrowUp.svg" alt="Arrow Up" class="arrow" @click="$emit('elevatorCalled','UP',props.floor)"/></div>
    <div><img src="../assets/arrowDown.svg" alt="Arrow Down" class="arrow" @click="$emit('elevatorCalled','DOWN',props.floor)"/></div>
  </div>
  <div class="optionGroup">
    <div>Elevator A:</div>
      <div v-if="elevatorADirection == 'up'">
      <img src="../assets/arrowUp.svg" alt="Arrow Up" class="arrow"/>
    </div>
    <div v-if="elevatorADirection == 'down'">
      <img src="../assets/arrowDown.svg" alt="Arrow Down" class="arrow"/>
    </div>
    <div v-if="elevatorADirection == 'here it is'">
      <img src="../assets/circle.svg" alt="Circle" class="arrow"/>
    </div>
  </div>
    
  <div class="optionGroup">
    <div>Elevator B:</div>
      <div v-if="elevatorBDirection == 'up'">
      <img src="../assets/arrowUp.svg" alt="Arrow Up" class="arrow"/>
    </div>
    <div v-if="elevatorBDirection == 'down'">
      <img src="../assets/arrowDown.svg" alt="Arrow Down" class="arrow"/>
    </div>
    <div v-if="elevatorBDirection == 'here it is'">
      <img src="../assets/circle.svg" alt="Circle" class="arrow"/>
    </div>
  </div>
    
  </div>
</template>

<style scoped>
.floor {
  width: 100%;
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
  gap: 1.5rem;
}

.arrow {
  height: 1.5rem;
  display: block;
  align-self: center;
  cursor: pointer;
}

.optionGroup {
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 0.5rem;
  width: 7rem;
}

.liftCallers {
  display: flex;
  flex-direction: row;
  gap: 0.5rem;
}
</style>
