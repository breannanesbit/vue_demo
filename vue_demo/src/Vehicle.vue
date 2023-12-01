<template>
    <div>

        <article class="icon" :style="{ left: `${props.x}px`, top:` ${props.y}px` }">
          <VehicleIcon
          :style=" {rotate:`${props.angle+90}deg`}"
          />
          <div>{{ props.name }}</div>
        </article>



    </div>
  </template>

<script setup lang="ts">
import VehicleIcon from './assets/VehicleIcon.vue';

  interface VehicleProps  {
    name: string;
    x: number;
    y: number;
    angle: number;
  }

  const props = defineProps<VehicleProps>();
  const emits = defineEmits();
</script>

<style scoped>
article {
    position: absolute;
}
</style>