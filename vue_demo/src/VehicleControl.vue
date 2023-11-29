
<template>
  <div>
    <img src="./rocket-svgrepo-com.svg" alt="rocket"/>
    <div v-for="player in players" :key="player.name">
      <div>{{ player.name }}</div>
      <div>{{ player.x }}, {{ player.y }}, {{ player.angle }}</div>
    </div>

    <!-- Register button -->
    <button @click="registerVehicle">Register Vehicle</button>
  </div>
</template>

<script lang="ts">
import { onUnmounted } from 'vue';
import { Player, directionOptions, getBoard, move, register } from './gameService';

export default {
  name: 'VehicleControl',
  data() {
    return {
      id: 0,
      players: [] as Player[],
    };
  },
  methods: {
    async registerVehicle() {
      this.id = await register('YourPlayerName'); // Adjust the name accordingly
    },
    async moveVehicle(direction: directionOptions) {
      await move(this.id, direction);
    },
  },
  async mounted() {
    // Fetch board data periodically
    const updateBoard = async () => {
      const board = await getBoard();
      this.players = board.players;
    };

    const controlKeys = {
        forwards: "w",
        backwards: "s",
        left: "a",
        right: "d",
      };
      
      const keydownListener = async (event: KeyboardEvent) => {
        if (event.key === controlKeys.forwards) await move(id, "Forward");
        if (event.key === controlKeys.backwards) await move(id, "Backward");
        if (event.key === controlKeys.left) await move(id, "Left");
        if (event.key === controlKeys.right) await move(id, "Right");
      };
      
      const keyupListener = async (event: KeyboardEvent) => {
        if (event.key === controlKeys.forwards) await move(id, "StopForward");
        if (event.key === controlKeys.backwards) await move(id, "StopBackward");
        if (event.key === controlKeys.left) await move(id, "StopLeft");
        if (event.key === controlKeys.right) await move(id, "StopRight");
      };
      document.addEventListener('keydown', keydownListener);

// Fetch initial board data
await updateBoard();

// Set up a periodic update
this.updateInterval = setInterval(updateBoard, 1000);

// Clean up listeners and interval on component unmount
onUnmounted(() => {
  document.removeEventListener('keydown', keydownListener);
  clearInterval(this.updateInterval);
});
},
};
</script>

<style scoped>
</style>
}