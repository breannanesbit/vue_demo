<template>
  <input v-model="name" placeholder="edit me" />
  <button @click="registerVehicle">Register Vehicle</button>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { register } from "./gameService";
const name = ref("");

const emit = defineEmits(['registorId'])

const registerVehicle = async () => {
  if (name.value) {
    const id = await register(name.value);
    emit('registorId', id)
  }
};
</script>
