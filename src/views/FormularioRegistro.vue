<template>
    <div class="max-w-4xl mx-auto py-12 px-6 bg-white shadow-lg rounded-lg">
      <h2 class="text-3xl font-extrabold mb-6 text-center text-gray-800">Registro</h2>
      <div class="mb-8">
        <div class="flex justify-between mb-4">
          <div :class="step >= 1 ? 'text-blue-600 font-semibold' : 'text-gray-400 font-normal'">Paso 1</div>
          <div :class="step >= 2 ? 'text-blue-600 font-semibold' : 'text-gray-400 font-normal'">Paso 2</div>
          <div :class="step >= 3 ? 'text-blue-600 font-semibold' : 'text-gray-400 font-normal'">Paso 3</div>
        </div>
        <div class="h-2 bg-gray-300 rounded-full overflow-hidden">
          <div :class="`w-${step * 1 / 3 * 100}% bg-blue-600 h-full`"></div>
        </div>
      </div>
      <div v-if="step === 1">
        <Paso1 v-bind="formStep1" @update="updateFormStep1" @next="nextStep" />
      </div>
      <div v-if="step === 2">
        <Paso2 v-bind="formStep2" @update="updateFormStep2" @prev="prevStep" @next="nextStep" />
        <button @click="resetForm" class="mt-4 p-3 bg-red-600 text-white font-semibold rounded hover:bg-red-700 transition duration-200">Borrar todo y volver al paso 1</button>
      </div>
      <div v-if="step === 3">
        <Paso3 v-bind="formStep3" @update="updateFormStep3" @prev="prevStep" @submit="submitForm" />
        <button @click="resetForm" class="mt-4 p-3 bg-red-600 text-white font-semibold rounded hover:bg-red-700 transition duration-200">Borrar todo y volver al paso 1</button>
      </div>
      <Modal v-if="modalVisible" @close="modalVisible = false">Formulario enviado con éxito</Modal>
    </div>
  </template>
  
  <script>
  import Paso1 from '@/components/PasoUnoComponente.vue'
  import Paso2 from '@/components/PasoDosComponente.vue'
  import Paso3 from '@/components/PasoTresComponente.vue'
  import Modal from '@/components/ModalComponent.vue'
  
  export default {
    components: {
      Paso1,
      Paso2,
      Paso3,
      Modal
    },
    data() {
      return {
        step: 1,
        modalVisible: false,
        formStep1: {
          pais: '',
          genero: '',
          primerNombre: '',
          segundoNombre: '',
          fechaNacimiento: '',
          tipoDocumento: '',
          numeroDocumento: '',
          fotoFrente: null,
          fotoReverso: null
        },
        formStep2: {
          email: '',
          password: '',
          confirmPassword: '',
          telefono: '',
          celular: ''
        },
        formStep3: {
          direccion: '',
          codigoPostal: ''
        }
      }
    },
    methods: {
      nextStep() {
        this.step++;
      },
      prevStep() {
        this.step--;
      },
      updateFormStep1(data) {
        this.formStep1 = data;
      },
      updateFormStep2(data) {
        this.formStep2 = data;
      },
      updateFormStep3(data) {
        this.formStep3 = data;
      },
      resetForm() {
        this.formStep1 = {
          pais: '',
          genero: '',
          primerNombre: '',
          segundoNombre: '',
          fechaNacimiento: '',
          tipoDocumento: '',
          numeroDocumento: '',
          fotoFrente: null,
          fotoReverso: null
        };
        this.formStep2 = {
          email: '',
          password: '',
          confirmPassword: '',
          telefono: '',
          celular: ''
        };
        this.formStep3 = {
          direccion: '',
          codigoPostal: ''
        };
        this.step = 1;
      },
      submitForm() {
        console.log({
          ...this.formStep1,
          ...this.formStep2,
          ...this.formStep3
        });
        this.modalVisible = true;
      }
    }
  }
  </script>
  
  <style scoped>
  /* Additional styles for modern look */
  .bg-white {
    background-color: #fff;
  }
  .shadow-lg {
    box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
  }
  .rounded-lg {
    border-radius: 0.5rem;
  }
  .font-extrabold {
    font-weight: 800;
  }
  .font-semibold {
    font-weight: 600;
  }
  .font-normal {
    font-weight: 400;
  }
  .text-gray-800 {
    color: #2d3748;
  }
  .text-blue-600 {
    color: #3182ce;
  }
  .text-gray-400 {
    color: #cbd5e0;
  }
  .h-2 {
    height: 0.5rem;
  }
  .bg-gray-300 {
    background-color: #e2e8f0;
  }
  .bg-blue-600 {
    background-color: #3182ce;
  }
  .bg-red-600 {
    background-color: #f56565;
  }
  .text-white {
    color: #fff;
  }
  .hover\:bg-red-700:hover {
    background-color: #e53e3e;
  }
  .transition {
    transition-property: all;
  }
  .duration-200 {
    transition-duration: 200ms;
  }
  </style>