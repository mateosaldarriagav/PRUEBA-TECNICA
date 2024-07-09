<template>
  <div class="max-w-2xl mx-auto p-8 bg-white shadow-lg rounded-lg">
    <form @submit.prevent="validateAndNext">
      <div class="mb-6">
        <label class="block text-gray-700 font-semibold mb-2">País</label>
        <select v-model="form.pais" @blur="validateField('pais')" @change="clearError('pais')" class="w-full p-2 border border-gray-300 rounded" :class="{'border-red-500': errors.pais}">
          <option value="" disabled>Selecciona un país</option>
          <option v-for="pais in paises" :key="pais.code" :value="pais.name">{{ pais.name }}</option>
        </select>
        <span v-if="errors.pais" class="text-red-500 text-sm">{{ errors.pais }}</span>
      </div>
      <div class="mb-6">
        <label class="block text-gray-700 font-semibold mb-2">Género</label>
        <select v-model="form.genero" @blur="validateField('genero')" @change="clearError('genero')" class="w-full p-2 border border-gray-300 rounded" :class="{'border-red-500': errors.genero}">
          <option value="" disabled>Selecciona tu género</option>
          <option value="masculino">Masculino</option>
          <option value="femenino">Femenino</option>
          <option value="otro">Otro</option>
        </select>
        <span v-if="errors.genero" class="text-red-500 text-sm">{{ errors.genero }}</span>
      </div>
      <div class="mb-6">
        <label class="block text-gray-700 font-semibold mb-2">Nombres</label>
        <input type="text" v-model="form.primerNombre" @blur="validateField('primerNombre')" @input="clearError('primerNombre')" class="w-full p-2 border border-gray-300 rounded" :class="{'border-red-500': errors.primerNombre}" />
        <span v-if="errors.primerNombre" class="text-red-500 text-sm">{{ errors.primerNombre }}</span>
      </div>
      <div class="mb-6">
        <label class="block text-gray-700 font-semibold mb-2">Apellidos</label>
        <input type="text" v-model="form.segundoNombre" @blur="validateField('segundoNombre')" @input="clearError('segundoNombre')" class="w-full p-2 border border-gray-300 rounded" :class="{'border-red-500': errors.segundoNombre}" />
        <span v-if="errors.segundoNombre" class="text-red-500 text-sm">{{ errors.segundoNombre }}</span>
      </div>
      <div class="mb-6">
        <label class="block text-gray-700 font-semibold mb-2">Fecha de nacimiento</label>
        <input type="date" v-model="form.fechaNacimiento" @blur="validateField('fechaNacimiento')" @input="clearError('fechaNacimiento')" class="w-full p-2 border border-gray-300 rounded" :class="{'border-red-500': errors.fechaNacimiento}" />
        <span v-if="errors.fechaNacimiento" class="text-red-500 text-sm">{{ errors.fechaNacimiento }}</span>
      </div>
      <div class="mb-6">
        <label class="block text-gray-700 font-semibold mb-2">Tipo de documento</label>
        <select v-model="form.tipoDocumento" @blur="validateField('tipoDocumento')" @change="clearError('tipoDocumento')" class="w-full p-2 border border-gray-300 rounded" :class="{'border-red-500': errors.tipoDocumento}">
          <option value="" disabled>Selecciona un tipo de documento</option>
          <option value="cc">Cédula de ciudadanía</option>
          <option value="pasaporte">Pasaporte</option>
          <option value="ppt">PPT - Permiso de protección temporal</option>
          <option value="ce">Cédula de extranjería</option>
        </select>
        <span v-if="errors.tipoDocumento" class="text-red-500 text-sm">{{ errors.tipoDocumento }}</span>
      </div>
      <div class="mb-6">
        <label class="block text-gray-700 font-semibold mb-2">Número de documento</label>
        <input type="text" v-model="form.numeroDocumento" @blur="validateField('numeroDocumento')" @input="clearError('numeroDocumento')" class="w-full p-2 border border-gray-300 rounded" :class="{'border-red-500': errors.numeroDocumento}" />
        <span v-if="errors.numeroDocumento" class="text-red-500 text-sm">{{ errors.numeroDocumento }}</span>
      </div>
      <div class="mb-6">
        <label class="block text-gray-700 font-semibold mb-2">Foto del documento - Frente</label>
        <input type="file" accept="image/jpeg" @change="onFileChange($event, 'frente')" class="w-full p-2 border border-gray-300 rounded" :class="{'border-red-500': errors.fotoFrente}" />
        <span v-if="errors.fotoFrente" class="text-red-500 text-sm">{{ errors.fotoFrente }}</span>
      </div>
      <div class="mb-6">
        <label class="block text-gray-700 font-semibold mb-2">Foto del documento - Reverso</label>
        <input type="file" accept="image/jpeg" @change="onFileChange($event, 'reverso')" class="w-full p-2 border border-gray-300 rounded" :class="{'border-red-500': errors.fotoReverso}" />
        <span v-if="errors.fotoReverso" class="text-red-500 text-sm">{{ errors.fotoReverso }}</span>
      </div>
      <button type="submit" class="w-full p-3 bg-blue-600 text-white font-semibold rounded hover:bg-blue-700 transition duration-200">Siguiente</button>
    </form>
  </div>
</template>

<script>
export default {
  props: ['pais', 'genero', 'primerNombre', 'segundoNombre', 'fechaNacimiento', 'tipoDocumento', 'numeroDocumento', 'fotoFrente', 'fotoReverso'],
  data() {
    return {
      form: {
        pais: this.pais,
        genero: this.genero,
        primerNombre: this.primerNombre,
        segundoNombre: this.segundoNombre,
        fechaNacimiento: this.fechaNacimiento,
        tipoDocumento: this.tipoDocumento,
        numeroDocumento: this.numeroDocumento,
        fotoFrente: this.fotoFrente,
        fotoReverso: this.fotoReverso
      },
      paises: [],
      errors: {
        pais: '',
        genero: '',
        primerNombre: '',
        segundoNombre: '',
        fechaNacimiento: '',
        tipoDocumento: '',
        numeroDocumento: '',
        fotoFrente: '',
        fotoReverso: ''
      } // Objeto para manejar errores individuales por campo
    };
  },
  mounted() {
    // Descargar lista de países
    fetch('https://restcountries.com/v3.1/all')
      .then(response => response.json())
      .then(data => {
        this.paises = data.map(country => ({ name: country.name.common, code: country.cca2 }));
      });
  },
  methods: {
    onFileChange(event, tipo) {
      const file = event.target.files[0];
      if (file && file.type === 'image/jpeg') {
        this.form[tipo === 'frente' ? 'fotoFrente' : 'fotoReverso'] = file;
        this.errors[tipo === 'frente' ? 'fotoFrente' : 'fotoReverso'] = '';
      } else {
        this.errors[tipo === 'frente' ? 'fotoFrente' : 'fotoReverso'] = 'Por favor, selecciona una imagen JPEG.';
      }
    },
    validateAndNext() {
      // Validar todos los campos antes de proceder
      this.validateField('pais');
      this.validateField('genero');
      this.validateField('primerNombre');
      this.validateField('segundoNombre');
      this.validateField('fechaNacimiento');
      this.validateField('tipoDocumento');
      this.validateField('numeroDocumento');

      

      // Validar fotos del documento
      if (!this.form.fotoFrente || !this.form.fotoReverso) {
        this.errors.fotoFrente = 'Por favor, adjunta una imagen válida para el frente del documento (JPG, JPEG y PNG).';
        this.errors.fotoReverso = 'Por favor, adjunta una imagen válida para el reverso del documento (JPG, JPEG y PNG).';
      }

      // Si no hay errores, proceder al siguiente paso
      if (this.checkFormValid()) {
        this.$emit('update', this.form);
        this.$emit('next');
      }
    },
    validateField(field) {
      switch (field) {
        case 'pais':
          this.errors.pais = this.form.pais ? '' : 'Por favor, selecciona un país.';
          break;
        case 'genero':
          this.errors.genero = this.form.genero ? '' : 'Por favor, selecciona un género.';
          break;
        case 'primerNombre':
          this.errors.primerNombre = this.validateNames(this.form.primerNombre) ? '' : 'Por favor, ingresa un nombre válido (solo letras y espacios).';
          break;
        case 'segundoNombre':
          this.errors.segundoNombre = this.validateNames(this.form.segundoNombre) ? '' : 'Por favor, ingresa un apellido válido (solo letras y espacios).';
          break;
        case 'fechaNacimiento':
          this.errors.fechaNacimiento = this.validateFechaNacimiento(this.form.fechaNacimiento) ? '' : 'Por favor, selecciona una fecha válida.';
          break;
        case 'tipoDocumento':
          this.errors.tipoDocumento = this.form.tipoDocumento ? '' : 'Por favor, selecciona un tipo de documento.';
          break;
        case 'numeroDocumento':
          this.errors.numeroDocumento = this.validateNumeroDocumento(this.form.numeroDocumento) ? '' : 'Por favor, ingresa un número de documento válido (entre 7 y 10 dígitos).';
          break;
        default:
          break;
      }
    },
    clearError(field) {
      this.errors[field] = '';
    },
    validateNames(name) {
      return /^[a-zA-ZáéíóúÁÉÍÓÚñÑ\s]+$/.test(name);
    },
    validateFechaNacimiento(date) {
      const selectedDate = new Date(date);
      const currentDate = new Date();
      return selectedDate < currentDate;
    },
    validateNumeroDocumento(numero) {
      return /^\d{7,10}$/.test(numero);
    },
    checkFormValid() {
      return Object.values(this.errors).every(error => error === '');
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
.text-white {
  color: #fff;
}
.hover\:bg-blue-700:hover {
  background-color: #2b6cb0;
}
.transition {
  transition-property: all;
}
.duration-200 {
  transition-duration: 200ms;
}
.border-red-500 {
  border-color: #e53e3e;
}
.text-red-500 {
  color: #e53e3e;
}
.text-sm {
  font-size: 0.875rem; /* Tamaño de fuente más pequeño para los mensajes de error */
}
</style>