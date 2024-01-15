<script setup>
import Header from './components/Header.vue'
import Formulario from './components/Formulario.vue'
import Paciente from './components/Paciente.vue'

import { reactive, ref, onMounted, watch } from 'vue'
import { uid } from 'uid'
let paciente = reactive({
  id: null,
  nombre: '',
  propietario: '',
  email: '',
  alta: '',
  sintomas: ''
})

const pacientes = ref([])

onMounted(() => {
  const pacientesLS = localStorage.getItem('pacientes')
  if (pacientesLS) {
    pacientes.value = JSON.parse(pacientesLS)
  }
})

watch(pacientes, () => {
  localStorage.setItem('pacientes', JSON.stringify(pacientes.value))
}, {
  deep: true
})

const guardarPaciente = () => {
  if (paciente.id) {
    const index = pacientes.value.findIndex(pacienteState => pacienteState.id === paciente.id)
    Object.assign(pacientes.value[index], paciente)
  } else {
    paciente.id = uid()
    pacientes.value.push({
      ...paciente
    })
  }

  //* Vaciar formulario
  paciente.id = null
  paciente.nombre = ''
  paciente.propietario = ''
  paciente.email = ''
  paciente.alta = ''
  paciente.sintomas = ''
}

const actualizarPaciente = id => {
  const pacienteObtenido = pacientes.value.filter(pacienteState => pacienteState.id === id)[0]
  Object.assign(paciente, pacienteObtenido)
}

const eliminarPaciente = id => {
  pacientes.value = pacientes.value.filter(pacienteState => pacienteState.id !== id)
}
</script>

<template>
  <div class="container mx-auto mt-20">
    <Header />
    <div class="mt-12 md:flex">
      <Formulario v-model:id="paciente.id" v-model:nombre="paciente.nombre" v-model:propietario="paciente.propietario"
        v-model:email="paciente.email" v-model:alta="paciente.alta" v-model:sintomas="paciente.sintomas"
        v-on:guardar-paciente="guardarPaciente" />
      <div class="md:w-1/2">
        <h3 class="font-black text-3xl text-center">Administra tus Pacientes</h3>
        <p class="text-lg mt-5 text-center">
          Información de pacientes
        </p>
        <div class="md:h-screen overflow-y-scroll">
          <p v-if="pacientes.length < 1" class="text-center mt-10 text-lg">No hay pacientes</p>
          <Paciente v-on:actualizar-paciente="actualizarPaciente" v-on:eliminar-paciente="eliminarPaciente"
            v-for="paciente in pacientes" v-bind:paciente="paciente" />
        </div>
      </div>
    </div>
  </div>
</template>
