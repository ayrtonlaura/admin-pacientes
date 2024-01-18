<script setup>
import { computed, ref, reactive } from 'vue';
import {uid} from 'uid'
import Formulario from './components/Formulario.vue';
import Paciente from './components/Paciente.vue'
import Header from './components/Header.vue';

// console.log('Random');
// console.log(Math.floor(Math.random() * 6));
// const dado = computed( () =>{
//   Math.floor(Math.random() * 6)
// })
const pacientes = ref([])
const paciente = reactive({
    id: null,
    nombre: '',
    propietario: '',
    email: '',
    alta: '',
    sintomas: ''
})


const guardarPaciente = () => {
  if(paciente.id){
    const {id} = paciente
    const i = pacientes.value.findIndex( (pacienteState) => pacienteState.id === id )
    pacientes.value[i] =   {...paciente}
    console.log(i);
  }else{
    pacientes.value.push({ 
      ...paciente, id: uid()
    })
  }
  
  
  Object.assign(paciente, {
    nombre: '',
    propietario: '',
    email: '',
    alta: '',
    sintomas: '',
    id: null
  })
}

const actualizarPaciente = (id) =>{
  const pacientesEditar = pacientes.value.filter( paciente => paciente.id === id)[0]
  Object.assign(paciente, pacientesEditar)
}
const eliminarPaciente = (id) =>{
  pacientes.value = pacientes.value.filter( paciente => paciente.id !== id)
}
</script>

<template>
  <div class="container mx-auto mt-20">
      <Header /> 
      <div class="mt-12 md:flex">
        <Formulario 
          v-model:nombre="paciente.nombre"
          v-model:propietario="paciente.propietario"
          v-model:email="paciente.email"
          v-model:alta="paciente.alta"
          v-model:sintomas="paciente.sintomas"
          @guardar-paciente="guardarPaciente"
          :id="paciente.id"
        />

        <div class="md:w-1/2 md:h-screen overflow-y-scroll">
          <h3 class="font-black text-3xl text-center">Administrar tus Pacientes</h3>
          <div v-if="pacientes.length > 0  ">
              <p class="text-lg mt-5 text-center mb-10">
                  Informacion de Pacientes 
                  <span class="text-indigo-600 font-bold">Administralos</span> 
              </p>    
              <Paciente 
                  v-for="(paciente,index) in pacientes" :key="index" :paciente="paciente"
                  @actualizar-paciente="actualizarPaciente"
                  @eliminar-paciente="eliminarPaciente"
                  />
          </div>
          <p v-else class="mt-20 text-2xl text-center">No Hay Pacientes</p>
        </div>
      </div>
  </div>
</template>


