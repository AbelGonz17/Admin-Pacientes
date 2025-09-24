<script setup>
import { ref, reactive } from "vue";
import { uid } from "uid";
import Formulario from "./components/Formulario.vue";
import Header from "./components/Header.vue";
import Paciente from "./components/Paciente.vue";

const pacientes = ref([]);

const paciente = reactive({
  id: null,
  nombre: "",
  propetario: "",
  email: "",
  alta: "",
  sintomas: "",
});

const guardarPaciente = () => {
  pacientes.value.push({ ...paciente, id: uid() });

  //reinicer el objeto

  paciente.nombre = "";
  paciente.propetario = "";
  paciente.email = "";
  paciente.alta = "";
  paciente.sintomas = "";
};

const actualizarPaciente = () => {
  console.log("actualizando..");
};

const eliminarPaciente = (id) => {
  pacientes.value = pacientes.value.filter((pc) => pc.id !== id);
};
</script>

<template>
  <div class="container mx-auto mt-20">
    <Header />

    <div class="mt-12 md:flex">
      <Formulario
        v-model:nombre="paciente.nombre"
        v-model:propetario="paciente.propetario"
        v-model:email="paciente.email"
        v-model:alta="paciente.alta"
        v-model:sintomas="paciente.sintomas"
        @guardar-paciente="guardarPaciente"
      />

      <div class="md:w-1/2 md:h-screen overflow-y-scrool">
        <h3 class="font-black text-3xl text-center">Adminitra tus pacientes</h3>
        <div v-if="pacientes.length > 0">
          <p class="text-lg mt-5 text-center mb-10">
            Informacion de
            <span class="text-indigo-600 font-bold">Pacientes</span>
          </p>
          <paciente
            v-for="paciente in pacientes"
            :paciente="paciente"
            @actualizar-paciente="actualizarPaciente"
            @eliminar-paciente="eliminarPaciente"
          ></paciente>
        </div>
        <p v-else class="mt-20 text-2xl text-center">No hay pacientes</p>
      </div>
    </div>
  </div>
</template>
