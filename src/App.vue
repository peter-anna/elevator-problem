<script setup lang="ts">
import { ref } from 'vue';
import ElevatorTemplate from './components/ElevatorTemplate.vue';
import Floor from './components/FloorTemplate.vue';
import { ELEVATOR_STATUS } from './constants/elevatorStatus';

interface Elevator {
  currentFloor: number;
  status: string;
}

const elevatorA = ref<Elevator>({
  currentFloor: 0,
  status: ELEVATOR_STATUS.DESTINATION_REACHED,
});

const elevatorB = ref<Elevator>({
  currentFloor: 6,
  status: ELEVATOR_STATUS.DESTINATION_REACHED,
});

function moveElevator(elevator: Elevator, to: number) {
  elevator.status = ELEVATOR_STATUS.MOVING;

  const interval = setInterval(() => {
    if (elevator.currentFloor < to) {
      elevator.currentFloor += 1;
    } else if (elevator.currentFloor > to) {
      elevator.currentFloor -= 1;
    }

    console.log(`Elevator is at floor ${elevator.currentFloor}`);

    if (elevator.currentFloor === to) {
      clearInterval(interval);
      elevator.status = ELEVATOR_STATUS.DESTINATION_REACHED;
      console.log(`Elevator reached floor ${to}`);
    }
  }, 1000);
}

function elevatorCalledEvent(direction: string, floor: number) {
  console.log(direction, 'called from floor ', floor)
  console.log(
    'A status =',
    elevatorA.value.status,
    ' floor =',
    elevatorA.value.currentFloor,
  )
  console.log(
    'B status =',
    elevatorB.value.status,
    ' floor =',
    elevatorB.value.currentFloor,
  )

  if (
    elevatorA.value.status == ELEVATOR_STATUS.DESTINATION_REACHED &&
    elevatorB.value.status == ELEVATOR_STATUS.DESTINATION_REACHED
  ) {
    const distanceA = Math.abs(elevatorA.value.currentFloor - floor)
    const distanceB = Math.abs(elevatorB.value.currentFloor - floor)

    if (distanceA < distanceB) {
      console.log('A is closer');
      elevatorA.value.status = ELEVATOR_STATUS.CALLED;
      moveElevator(elevatorA.value, floor);
    } else if (distanceA > distanceB) {
      console.log('B is closer');
      elevatorB.value.status = ELEVATOR_STATUS.CALLED;
      moveElevator(elevatorB.value, floor);
    } else {
      console.log('Equal distance');
      if (elevatorA.value.currentFloor < elevatorB.value.currentFloor) {
        console.log('A will go');
        elevatorA.value.status = ELEVATOR_STATUS.CALLED;
        moveElevator(elevatorA.value, floor);
      } else {
        console.log('B will go');
        elevatorB.value.status = ELEVATOR_STATUS.CALLED;
        moveElevator(elevatorB.value, floor);
      }
    }
  }
}

function destinationSelectedEvent(
  elevator: string,
  destination: number,
  currentFloor: number,
) {
  console.log(
    'elevator ',
    elevator,
    ' destination is ',
    destination,
    ' from floor ',
    currentFloor,
  )
  if (elevator == 'A') {
    elevatorA.value.status = ELEVATOR_STATUS.DESTINATION_SELECTED;
    moveElevator(elevatorA.value, destination);
  } else {
    elevatorB.value.status = ELEVATOR_STATUS.DESTINATION_SELECTED;
    moveElevator(elevatorB.value, destination);
  }
 
}
</script>

<template>
  <div class="content">
    <Floor :floor="6" @elevatorCalled="elevatorCalledEvent" />
    <Floor :floor="5" @elevatorCalled="elevatorCalledEvent" />
    <Floor :floor="4" @elevatorCalled="elevatorCalledEvent" />
    <Floor :floor="3" @elevatorCalled="elevatorCalledEvent" />
    <Floor :floor="2" @elevatorCalled="elevatorCalledEvent" />
    <Floor :floor="1" @elevatorCalled="elevatorCalledEvent" />
    <Floor :floor="0" @elevatorCalled="elevatorCalledEvent" />
  </div>
  <div class="lifts">
    <ElevatorTemplate
      name="A"
      :currentFloor="elevatorA.currentFloor"
      :status="elevatorA.status"
      @destinationSelected="destinationSelectedEvent"
    />
    <ElevatorTemplate
      name="B"
      :currentFloor="elevatorB.currentFloor"
      :status="elevatorB.status"
      @destinationSelected="destinationSelectedEvent"
    />
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
