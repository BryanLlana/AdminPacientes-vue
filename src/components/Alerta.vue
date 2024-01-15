<script setup>
const props = defineProps({
  alerta: {
    type: Object,
    required: true
  }
})
</script>

<template>
  <div
    v-bind:class="[alerta.tipo === 'error' ? 'bg-red-500' : 'bg-green-500']"
    class="mb-5 p-2 text-center text-white font-bold text-lg rounded-md">
    {{ alerta.mensaje }}
  </div>
</template>
