<script setup lang="ts">
import { ref } from 'vue'
import ElevatorTemplate from './components/ElevatorTemplate.vue'
import Floor from './components/FloorTemplate.vue'
import { ELEVATOR_STATUS } from './constants/elevatorStatus'
import type { Elevator } from './types/elevator';

const elevatorCallQueue = ref<Array<{ direction: string, floor: number }>>([]);

const elevatorA = ref<Elevator>({
  currentFloor: 0,
  status: ELEVATOR_STATUS.DESTINATION_REACHED,
  name: 'A'
})

const elevatorB = ref<Elevator>({
  currentFloor: 6,
  status: ELEVATOR_STATUS.DESTINATION_REACHED,
  name: 'B'
})

function moveElevator(elevator: Elevator, to: number) {
  elevator.status = ELEVATOR_STATUS.CALLED

  const interval = setInterval(() => {
    if (elevator.currentFloor < to) {
      elevator.currentFloor += 1
    } else if (elevator.currentFloor > to) {
      elevator.currentFloor -= 1
    }

    if (elevator.currentFloor === to) {
      clearInterval(interval)
      elevator.status = ELEVATOR_STATUS.DESTINATION_REACHED

      setTimeout(() => {
        elevatorReachedDestination(elevator);  // process any pending external requests
      }, 5000);

    }
  }, 1000)
}

function elevatorCalledEvent(direction: string, floor: number) {

  let comparisonResult = 'both_moving'
  const distanceA = Math.abs(elevatorA.value.currentFloor - floor)
  const distanceB = Math.abs(elevatorB.value.currentFloor - floor)

   // check if either or both elevators are free
  if (
    elevatorA.value.status === ELEVATOR_STATUS.DESTINATION_REACHED &&
    elevatorB.value.status === ELEVATOR_STATUS.DESTINATION_REACHED
  ) {
    if (distanceA < distanceB) {
      comparisonResult = 'A_closer'
    } else if (distanceB < distanceA) {
      comparisonResult = 'B_closer'
    } else {
      comparisonResult = 'equal_distance'
    }
  } else if (elevatorA.value.status === ELEVATOR_STATUS.DESTINATION_REACHED) {
    comparisonResult = 'A_free'
  } else if (elevatorB.value.status === ELEVATOR_STATUS.DESTINATION_REACHED) {
    comparisonResult = 'B_free'
  }

  switch (comparisonResult) {
    case 'A_closer':
      elevatorA.value.status = ELEVATOR_STATUS.CALLED
      moveElevator(elevatorA.value, floor)
      break
    case 'B_closer':
      elevatorB.value.status = ELEVATOR_STATUS.CALLED
      moveElevator(elevatorB.value, floor)
      break
    case 'equal_distance':
      if (elevatorA.value.currentFloor < elevatorB.value.currentFloor) {
        elevatorA.value.status = ELEVATOR_STATUS.CALLED
        moveElevator(elevatorA.value, floor)
      } else if (elevatorB.value.currentFloor < elevatorA.value.currentFloor){
        elevatorB.value.status = ELEVATOR_STATUS.CALLED
        moveElevator(elevatorB.value, floor)
      } else {
        // the lifts are on the same floor
        const randomChoice = Math.random() < 0.5 ? 'A' : 'B';
    
        if (randomChoice === 'A') {
          elevatorA.value.status = ELEVATOR_STATUS.CALLED;
          moveElevator(elevatorA.value, floor);
        } else {
          elevatorB.value.status = ELEVATOR_STATUS.CALLED;
          moveElevator(elevatorB.value, floor);
        }
      }
      break
    case 'A_free':
      elevatorA.value.status = ELEVATOR_STATUS.CALLED
      moveElevator(elevatorA.value, floor)
      break

    case 'B_free':
      elevatorB.value.status = ELEVATOR_STATUS.CALLED
      moveElevator(elevatorB.value, floor)
      break

    default:
      // if both are busy, queue the request for later processing
      elevatorCallQueue.value.push({ direction, floor });
      break
  }
}

function elevatorReachedDestination(elevator: Elevator) {
  elevator.status = ELEVATOR_STATUS.DESTINATION_REACHED;
  setTimeout(() => {
  // check the external queue for any pending passenger requests (lift caller buttons)
  if (elevatorCallQueue.value.length > 0) {
    const nextCall = elevatorCallQueue.value.shift();
    if (nextCall) {
      elevator.status = ELEVATOR_STATUS.CALLED;
      moveElevator(elevator, nextCall.floor);
    }
  }

}, 2000);

}


function destinationSelectedEvent(
  elevatorName: string,
  destination: number,
) {
  const elevator = elevatorName === 'A' ? elevatorA.value : elevatorB.value;

  if (elevator.status === ELEVATOR_STATUS.CALLED) {
  
   return;
  } else {
    
     // if the elevator is steady, move it to the requested destination
    elevator.status = ELEVATOR_STATUS.CALLED;
    moveElevator(elevator, destination);
  }
 
}
</script>

<template>
  <div class="content">
    <Floor :floor="6" :elevatorA="elevatorA" :elevatorB="elevatorB" @elevatorCalled="elevatorCalledEvent" />
    <Floor :floor="5" :elevatorA="elevatorA" :elevatorB="elevatorB" @elevatorCalled="elevatorCalledEvent" />
    <Floor :floor="4" :elevatorA="elevatorA" :elevatorB="elevatorB" @elevatorCalled="elevatorCalledEvent" />
    <Floor :floor="3" :elevatorA="elevatorA" :elevatorB="elevatorB" @elevatorCalled="elevatorCalledEvent" />
    <Floor :floor="2" :elevatorA="elevatorA" :elevatorB="elevatorB" @elevatorCalled="elevatorCalledEvent" />
    <Floor :floor="1" :elevatorA="elevatorA" :elevatorB="elevatorB" @elevatorCalled="elevatorCalledEvent" />
    <Floor :floor="0" :elevatorA="elevatorA" :elevatorB="elevatorB" @elevatorCalled="elevatorCalledEvent" />
    
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
