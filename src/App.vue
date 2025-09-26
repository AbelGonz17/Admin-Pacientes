<script setup>
import { ref, reactive, watch, onMounted } from "vue";
import { uid } from "uid";
import Formulario from "./components/Formulario.vue";
import Header from "./components/Header.vue";
import Paciente from "./components/Paciente.vue";

const pacientes = ref([]);

const pacienteInicial = {
  id: null,
  nombre: "",
  propietario: "",
  email: "",
  alta: "",
  sintomas: "",
};

const paciente = reactive({
  id: null,
  nombre: "",
  propietario: "",
  email: "",
  alta: "",
  sintomas: "", 
});

watch(
  pacientes,
  () => {
    guadarLocalStorage();
  },
  {
    deep: true,
  }
);

onMounted(() => {
  const pacienteStorage = localStorage.getItem("pacientes");
  if (pacienteStorage) {
    pacientes.value = JSON.parse(pacienteStorage);
  }
});

const guadarLocalStorage = () => {
  localStorage.setItem("pacientes", JSON.stringify(pacientes.value));
};

const guardarPaciente = () => {
  if (paciente.id) {
    const { id } = paciente;
    const i = pacientes.value.findIndex((pc) => pc.id === id);
    pacientes.value[i] = { ...paciente };
  } else {
    pacientes.value.push({ ...paciente, id: uid() });
  }

  //reinicer el objeto
  Object.assign(paciente, pacienteInicial);
};

const actualizarPaciente = (id) => {
  const pacienteEditar = pacientes.value.find((pc) => pc.id === id);
  Object.assign(paciente, pacienteEditar);
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
        v-model:propietario="paciente.propietario"
        v-model:email="paciente.email"
        v-model:alta="paciente.alta"
        v-model:sintomas="paciente.sintomas"
        @guardar-paciente="guardarPaciente"
        :id="paciente.id"
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
