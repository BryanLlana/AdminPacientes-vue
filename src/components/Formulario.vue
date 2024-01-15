<script setup>
import Alerta from './Alerta.vue'
import { reactive } from 'vue'

const alerta = reactive({
  mensaje: '',
  tipo: ''
})

const $emit = defineEmits(['update:id', 'update:nombre', 'update:propietario', 'update:email', 'update:alta', 'update:sintomas', 'guardar-paciente'])

const props = defineProps({
  id: {
    type: null || String,
    required: true
  },
  nombre: {
    type: String,
    required: true
  },
  propietario: {
    type: String,
    required: true
  },
  email: {
    type: String,
    required: true
  },
  alta: {
    type: String,
    required: true
  },
  sintomas: {
    type: String,
    required: true
  }
})

const validar = () => {
  if (Object.values(props).includes('')) {
    alerta.mensaje = 'Todos los campos son obligatorios'
    alerta.tipo = 'error'

    setTimeout(() => {
      alerta.mensaje = ''
      alerta.tipo = ''
    }, 3000)

    return
  }
  const idExiste = props.id
  $emit('guardar-paciente')
  if (idExiste) {
    alerta.mensaje = 'Paciente modificado correctamente'
  } else {
    alerta.mensaje = 'Paciente guardado correctamente'
  }
  alerta.tipo = 'exito'
  setTimeout(() => {
    alerta.mensaje = ''
    alerta.tipo = ''
  }, 3000)
}
</script>

<template>
  <div class="md:w-1/2">
    <h2 class="font-black text-3xl text-center">Seguimiento Pacientes</h2>
    <p class="text-lg mt-5 text-center mb-10">
      Añade Pacientes y
      <span class="text-indigo-600 font-bold">Administralos</span>
    </p>

    <Alerta v-if="alerta.mensaje" v-bind:alerta="alerta" />

    <form v-on:submit.prevent="validar" class="bg-white shadow-md rounded-lg py-10 px-5 mb-10">
      <div class="mb-5">
        <label for="mascota" class="block text-gray-700 uppercase font-bold">Nombre Mascota</label>
        <input v-bind:value="nombre" v-on:input="$emit('update:nombre', $event.target.value)" type="text" id="mascota"
          placeholder="Ejm: Chanchito" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md">
      </div>
      <div class="mb-5">
        <label for="propietario" class="block text-gray-700 uppercase font-bold">Nombre Propietario</label>
        <input v-bind:value="propietario" v-on:input="$emit('update:propietario', $event.target.value)" type="text"
          id="propietario" placeholder="Ejm: Juan" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md">
      </div>
      <div class="mb-5">
        <label for="email" class="block text-gray-700 uppercase font-bold">Email</label>
        <input v-bind:value="email" v-on:input="$emit('update:email', $event.target.value)" type="email" id="email"
          placeholder="Ejm: juan@gmail.com" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md">
      </div>
      <div class="mb-5">
        <label for="alta" class="block text-gray-700 uppercase font-bold">Alta</label>
        <input v-bind:value="alta" v-on:input="$emit('update:alta', $event.target.value)" type="date" id="alta"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md">
      </div>
      <div class="mb-5">
        <label for="sintomas" class="block text-gray-700 uppercase font-bold">Síntomas</label>
        <textarea v-bind:value="sintomas" v-on:input="$emit('update:sintomas', $event.target.value)" id="sintomas"
          placeholder="Ejm: Dolor de cabeza" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-40" />
      </div>
      <input type="submit" v-bind:value="[id ? 'Modificar Paciente' : 'Guardar paciente']"
        class="bg-indigo-600 w-full p-3 text-white uppercase font-bold hover:bg-indigo-700 cursor-pointer trasnsition-colors">
    </form>
  </div>
</template>
