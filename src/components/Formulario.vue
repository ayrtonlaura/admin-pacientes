<script setup>
import { reactive, ref, computed } from 'vue';
import Alerta from './Alerta.vue';
// const dado = computed( () =>{
//   return Math.floor(Math.random() * 7)
// })
const alerta = reactive({
    mensaje: '',
    tipo: ''
})

const emit = defineEmits(['update:nombre','update:propietario','update:email', 'update:alta', 'update:sintomas', 'guardar-paciente'])

const props = defineProps(['id','nombre','propietario','email','alta','sintomas'])
const validar = () =>{
    if(Object.values(props).includes('')){
        alerta.mensaje = 'Todos los campos son obligarios'
        alerta.tipo = 'error'
        return
    }
    emit('guardar-paciente')
    alerta.mensaje = 'Registrado Correctamente'
    alerta.tipo = 'exito'
    
    setTimeout( ()=>{
        Object.assign(alerta, {
            mensaje: '',
            tipo: ''
        })
    },3000)
}

const editando = computed( () =>{
    return props.id
})
</script>

<template>
    <div class="md:w-1/2">
        <h2 class="font-black text-3xl text-center">Seguimiento Pacientes</h2>
        <p class="text-lg mt-5 text-center mb-10">
            Añade Pacientes y 
            <span class="text-indigo-600 font-bold">Administralos</span> 
        </p>
        <Alerta v-if="alerta.mensaje" :alerta="alerta"/>
        <form class="bg-white shadow-md rounded-lg py-10 px-5 mb-10" @submit.prevent="validar()">
            <div class="mb-5">
                <label 
                        for="mascota"
                        class="block text-gray-700 uppercase font-bold"
                >
                    Nombre Mascota
                </label>
                <input 
                        type="text"
                        id="mascota"
                        @input="$emit('update:nombre', $event.target.value)"
                        :value="nombre"
                        placeholder="Nombre de la mascota"
                        class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                />
            </div>
            <div class="mb-5">
                <label 
                        for="propietario"
                        class="block text-gray-700 uppercase font-bold"
                >
                    Nombre Propietario
                </label>
                <input 
                        type="text"
                        id="propietario"
                        @input="$emit('update:propietario', $event.target.value)"
                        :value="propietario"
                        placeholder="Nombre del propietario"
                        class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                />
            </div>
            <div class="mb-5">
                <label 
                        for="email"
                        class="block text-gray-700 uppercase font-bold"
                >
                    Email
                </label>
                <input 
                        type="text"
                        id="email"
                        :value="email"
                        @input="$emit('update:email', $event.target.value)"
                        placeholder="email del propietario"
                        class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                />
            </div>
            <div class="mb-5">
                <label 
                        for="alta"
                        class="block text-gray-700 uppercase font-bold"
                >
                    Alta
                </label>
                <input 
                        type="date"
                        id="alta"
                        :value="alta"
                        @input="$emit('update:alta', $event.target.value)"
                        class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                />
            </div>
            <div class="mb-5">
                <label 
                        for="sintomas"
                        class="block text-gray-700 uppercase font-bold"
                >
                    Sintomas
                </label>
                <textarea
                        
                        id="sintomas"
                        @input="$emit('update:sintomas', $event.target.value)"
                        :value="sintomas"
                        placeholder="Describe los sintomas"
                        class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-40"
                />
            </div>
            <input type="submit" class="bg-indigo-600 w-full p-3 text-white uppercase font-bold hover:bg-indigo cursor-pointer transition-colors"
                    :value="[editando ? 'Guardar Cambios' : 'Registrar Paciente']"
            >
        </form>
    </div>
</template>