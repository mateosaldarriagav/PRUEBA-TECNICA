<template>
  <div class="max-w-2xl mx-auto p-8 bg-white shadow-lg rounded-lg">
    <form @submit.prevent="submitForm">
      <div class="mb-6">
        <label class="block text-gray-700 font-semibold mb-2">Dirección</label>
        <input type="text" v-model="form.direccion" @blur="validateDireccion" @input="clearError('direccion')"
               class="w-full p-2 border border-gray-300 rounded"
               :class="{'border-red-500': !isValidDireccion && (formSubmitted || direccionBlurred)}" />
        <span v-if="!isValidDireccion && (formSubmitted || direccionBlurred)" class="text-red-500 text-sm block">Por favor, ingresa una dirección válida.</span>
      </div>
      <div class="mb-6">
        <label class="block text-gray-700 font-semibold mb-2">Código postal</label>
        <input type="text" v-model="form.codigoPostal" @blur="validateCodigoPostal" @input="clearError('codigoPostal')"
               pattern="[0-9]{6}" title="El código postal debe ser un número de 6 dígitos"
               class="w-full p-2 border border-gray-300 rounded"
               :class="{'border-red-500': !isValidCodigoPostal && (formSubmitted || codigoPostalBlurred)}" />
        <span v-if="!isValidCodigoPostal && (formSubmitted || codigoPostalBlurred)" class="text-red-500 text-sm block">Por favor, ingresa un código postal válido (6 dígitos numéricos).</span>
      </div>
      <div class="flex justify-between">
        <button type="button" @click="$emit('prev')"
                class="w-1/3 p-3 bg-gray-600 text-white font-semibold rounded hover:bg-gray-700 transition duration-200">Anterior</button>
        <button type="button"
                class="w-1/3 p-3 bg-blue-600 text-white font-semibold rounded hover:bg-blue-700 transition duration-200"
                @click="submitForm">Enviar</button>
      </div>
    </form>

    <!-- Modal -->
    <div class="fixed top-0 left-0 w-full h-full flex items-center justify-center z-50" v-if="isValidForm">
      <div class="absolute w-full h-full bg-gray-900 opacity-75"></div>
      <div class="bg-white p-8 rounded shadow-lg z-10">
        <p class="text-green-500 text-lg mb-4">Formulario enviado correctamente.</p>
        <button @click="closeModal"
                class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600 transition duration-200">Cerrar</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['direccion', 'codigoPostal'],
  data() {
    return {
      form: {
        direccion: this.direccion,
        codigoPostal: this.codigoPostal
      },
      isValidDireccion: true,
      isValidCodigoPostal: true,
      formSubmitted: false,
      isValidForm: false, // Nuevo estado para validar si el formulario es válido
      direccionBlurred: false,
      codigoPostalBlurred: false
    };
  },
  methods: {
    validateDireccion() {
      this.isValidDireccion = this.form.direccion.trim() !== '';
      this.direccionBlurred = true;
    },
    validateCodigoPostal() {
      this.isValidCodigoPostal = /^\d{6}$/.test(this.form.codigoPostal);
      this.codigoPostalBlurred = true;
    },
    submitForm() {
      this.validateDireccion();
      this.validateCodigoPostal();

      // Si todo está válido, emitir evento para actualizar y marcar el formulario como válido
      if (this.isValidDireccion && this.isValidCodigoPostal) {
        this.$emit('update', this.form);
        this.isValidForm = true; // Marcar el formulario como válido
        // Aquí deberías enviar el formulario o realizar la acción necesaria
        // this.$emit('submit');
      }

      this.formSubmitted = true;
    },
    closeModal() {
      this.isValidForm = false; // Cerrar la modal
      this.formSubmitted = false; // Reiniciar el estado del formulario
      this.direccionBlurred = false; // Reiniciar estado de blur de dirección
      this.codigoPostalBlurred = false; // Reiniciar estado de blur de código postal
    },
    clearError(field) {
      switch (field) {
        case 'direccion':
          this.isValidDireccion = true;
          break;
        case 'codigoPostal':
          this.isValidCodigoPostal = true;
          break;
        default:
          break;
      }
    }
  }
};
</script>

<style scoped>
/* Estilos adicionales para un aspecto moderno */
.bg-white {
  background-color: #fff;
}
.shadow-lg {
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
}
.rounded-lg {
  border-radius: 0.5rem;
}
.font-semibold {
  font-weight: 600;
}
.text-gray-700 {
  color: #4a5568;
}
.border-gray-300 {
  border-color: #e2e8f0;
}
.bg-blue-600 {
  background-color: #3182ce;
}
.bg-gray-600 {
  background-color: #718096;
}
.text-white {
  color: #fff;
}
.text-red-500 {
  color: #e53e3e;
}
.text-green-500 {
  color: #48bb78;
}
.text-sm {
  font-size: 0.875rem; /* Tamaño de fuente más pequeño para los mensajes de error */
}
.hover\:bg-blue-700:hover {
  background-color: #2b6cb0;
}
.hover\:bg-gray-700:hover {
  background-color: #4a5568;
}
.transition {
  transition-property: all;
}
.duration-200 {
  transition-duration: 200ms;
}
</style>