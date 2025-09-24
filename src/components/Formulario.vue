<script setup>
import { reactive } from "vue";
import Alerta from "./Alerta.vue";

const alerta = reactive({
  tipo: "",
  mensaje: "",
});

const emit = defineEmits([
  "update:nombre",
  "update:propetario",
  "update:email",
  "update:alta",
  "update:sintomas",
  "guardar-paciente",
]);

const props = defineProps({
  nombre: {
    type: String,
    requiered: true,
  },
  propetario: {
    type: String,
    requiered: true,
  },
  email: {
    type: String,
    requiered: true,
  },
  alta: {
    type: String,
    required: true,
  },
  sintomas: {
    type: String,
    required: true,
  },
});

const validar = () => {
  if (Object.values(props).includes("")) {
    alerta.mensaje = "todos los campos son obligatorios";
    alerta.tipo = "error";
    return;
  }
  emit("guardar-paciente");
  alerta.mensaje = "paciente Almanecenado Correctamente";
  alerta.tipo = "exito";

    setTimeout(() => {
        Object.assign(alerta,{tipo:'',mensaje:''})
    }, 3000);
};
</script>

<template>
  <div class="md:w-1/2">
    <h2 class="font-black text-3xl text-center">Seguimiento Pacientes</h2>

    <p class="text-lg mt-5 text-center mb-10">
      agregar Pacientes y
      <span class="text-indigo-600 font-bold">Administralos</span>
    </p>
    <Alerta v-if="alerta.mensaje" :alerta="alerta"></Alerta>
    <form
      class="bg-white shadow-md rounded-lg py-10 px-5 mb=10"
      @submit.prevent="validar"
    >
      <div class="mb-5">
        <label for="mascota" class="block text-gray-700 uppercase font-bold">
          Nombre Macosta
        </label>
        <input
          id="mascota"
          type="text"
          placeholder="Nombre de la mascota"
          class="border-2 w-full mt-2 placeholder-gray-400 rounded-md"
          :value="nombre"
          @input="$emit('update:nombre', $event.target.value)"
        />
      </div>

      <div class="mb-5">
        <label for="propetario" class="block text-gray-700 uppercase font-bold">
          Nombre Propetario
        </label>
        <input
          id="propetario"
          type="text"
          placeholder="Nombre del propetario "
          class="border-2 w-full mt-2 placeholder-gray-400 rounded-md"
          :value="propetario"
          @input="$emit('update:propetario', $event.target.value)"
        />
      </div>

      <div class="mb-5">
        <label for="email" class="block text-gray-700 uppercase font-bold">
          Email Contacto
        </label>
        <input
          id="email"
          type="email"
          placeholder="Email Propetario"
          class="border-2 w-full mt-2 placeholder-gray-400 rounded-md"
          :value="email"
          @input="$emit('update:email', $event.target.value)"
        />
      </div>

      <div class="mb-5">
        <label for="alta" class="block text-gray-700 uppercase font-bold">
          Alta
        </label>
        <input
          id="alta"
          type="date"
          placeholder="Fecha de alta"
          class="border-2 w-full mt-2 placeholder-gray-400 rounded-md"
          :value="alta"
          @input="$emit('update:alta', $event.target.value)"
        />
      </div>

      <div class="mb-5">
        <label for="sintomas" class="block text-gray-700 uppercase font-bold">
          Sintomas de la mascota
        </label>
        <textarea
          id="sintomas"
          placeholder="describe los sintomas de este paciente"
          class="border-2 w-full mt-2 placeholder-gray-400 rounded-md h-40"
          :value="sintomas"
          @input="$emit('update:sintomas', $event.target.value)"
        />
      </div>

      <input
        type="submit"
        class="bg-indigo-600 w-full p-3 text-white uppercase font-bold hover:bg-indigo-700 cursor-pointer transition-colors"
        value="Registrar paciente"
      />
    </form>
  </div>
</template>
