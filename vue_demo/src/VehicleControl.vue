<template>
  <div>
    <Registor @registorId="registerVehicle" />
    <div v-if="players.length > 0">
      <Vehicle 
      v-for="player in players"
      :key="player.name"
      :name="player.name"
      :x="player.x"
      :y="player.y"
      :angle="player.angle"
       />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';
import Vehicle from './Vehicle.vue';
import { Player, directionOptions, getBoard, move } from './gameService';
import Registor from './Registor.vue';

const id = ref(0);
const players = ref([] as Player[]);
const updateInterval = ref(0)

const controlKeys = {
  forwards: 'w',
  backwards: 's',
  left: 'a',
  right: 'd',
};

const registerVehicle = async (playerid: number) => {
    id.value = playerid
};

const moveVehicle = async (direction: directionOptions) => {
  await move(id.value, direction);
};

const keydownListener = async (event: KeyboardEvent) => {
  if (event.key === controlKeys.forwards) await moveVehicle('Forward');
  if (event.key === controlKeys.backwards) await moveVehicle('Backward');
  if (event.key === controlKeys.left) await moveVehicle('Left');
  if (event.key === controlKeys.right) await moveVehicle('Right');
};

const keyupListener = async (event: KeyboardEvent) => {
  if (event.key === controlKeys.forwards) await moveVehicle('StopForward');
  if (event.key === controlKeys.backwards) await moveVehicle('StopBackward');
  if (event.key === controlKeys.left) await moveVehicle('StopLeft');
  if (event.key === controlKeys.right) await moveVehicle('StopRight');
};

onMounted(() => {
  document.addEventListener('keydown', keydownListener);
  document.addEventListener('keyup', keyupListener);
  updateInterval.value = setInterval(updateBoard, 1000);
});

onUnmounted(() => {
  document.removeEventListener('keydown', keydownListener);
  document.removeEventListener('keyup', keyupListener);
  clearInterval(updateInterval.value);
});

const updateBoard = async () => {
  const board = await getBoard();
  players.value = board.players;
};
</script>

<style scoped>
  .icon {
    /* Your styling here */
  }
</style>
