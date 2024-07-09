<template>
  <div class="max-w-2xl mx-auto p-8 bg-white shadow-lg rounded-lg">
    <form @submit.prevent="validateAndNext">
      <div class="mb-6">
        <label class="block text-gray-700 font-semibold mb-2">Correo electrónico</label>
        <input type="email" v-model="form.email" @blur="validateField('email')" class="w-full p-2 border border-gray-300 rounded"
               :class="{'border-red-500': formErrors.email}" />
        <span v-if="formErrors.email" class="text-red-500 text-sm">{{ formErrors.email }}</span>
      </div>
      <div class="mb-6">
        <label class="block text-gray-700 font-semibold mb-2">Contraseña</label>
        <input type="password" v-model="form.password" @blur="validateField('password')" class="w-full p-2 border border-gray-300 rounded"
               :class="{'border-red-500': formErrors.password}" />
        <span v-if="formErrors.password" class="text-red-500 text-sm">{{ formErrors.password }}</span>
      </div>
      <div class="mb-6">
        <label class="block text-gray-700 font-semibold mb-2">Confirmar contraseña</label>
        <input type="password" v-model="form.confirmPassword" @blur="validateField('confirmPassword')" class="w-full p-2 border border-gray-300 rounded"
               :class="{'border-red-500': formErrors.confirmPassword}" />
        <span v-if="formErrors.confirmPassword" class="text-red-500 text-sm">{{ formErrors.confirmPassword }}</span>
      </div>
      <div class="mb-6">
        <label class="block text-gray-700 font-semibold mb-2">Teléfono</label>
        <input type="tel" v-model="form.telefono" @blur="validateField('telefono')" pattern="[0-9]{10}" title="El teléfono debe tener exactamente 10 dígitos" class="w-full p-2 border border-gray-300 rounded"
               :class="{'border-red-500': formErrors.telefono}" />
        <span v-if="formErrors.telefono" class="text-red-500 text-sm">{{ formErrors.telefono }}</span>
      </div>
      <div class="mb-6">
        <label class="block text-gray-700 font-semibold mb-2">Celular</label>
        <div class="flex items-center">
          <select v-model="form.countryCode" class="p-2 border border-gray-300 rounded-l">
            <option v-for="code in countryCodes" :key="code" :value="code">{{ code }}</option>
          </select>
          <input type="tel" v-model="form.celular" @blur="validateField('celular')" class="w-full p-2 border border-gray-300 rounded-r"
                 :class="{'border-red-500': formErrors.celular}" />
        </div>
        <span v-if="formErrors.celular" class="text-red-500 text-sm">{{ formErrors.celular }}</span>
      </div>
      <div class="flex justify-between">
        <button type="button" @click="previousStep" class="w-1/3 p-3 bg-gray-600 text-white font-semibold rounded hover:bg-gray-700 transition duration-200">Anterior</button>
        <button type="submit" class="w-1/3 p-3 bg-blue-600 text-white font-semibold rounded hover:bg-blue-700 transition duration-200">Siguiente</button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  props: ['email', 'password', 'confirmPassword', 'telefono', 'celular', 'countryCode'],
  data() {
    return {
      form: {
        email: this.email,
        password: this.password,
        confirmPassword: this.confirmPassword,
        telefono: this.telefono,
        celular: this.celular,
        countryCode: this.countryCode || '+57' // código por defecto
      },
      formErrors: {},
      formSubmitted: false,
      countryCodes: ['+1', '+44', '+33', '+49', '+57', '+91'] // lista de indicativos de países
    };
  },
  methods: {
    validateEmail() {
      const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      return emailRegex.test(this.form.email);
    },
    validatePassword() {
      const passwordRegex = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$/;
      return passwordRegex.test(this.form.password);
    },
    validateConfirmPassword() {
      return this.form.password === this.form.confirmPassword;
    },
    validateTelefono() {
      return /^\d{10}$/.test(this.form.telefono);
    },
    validateCelular() {
      return /^\d{10}$/.test(this.form.celular);
    },
    validateField(field) {
      switch (field) {
        case 'email':
          this.formErrors.email = !this.validateEmail() ? 'Por favor, ingresa un correo electrónico válido.' : '';
          break;
        case 'password':
          this.formErrors.password = !this.validatePassword() ? 'La contraseña debe tener al menos 8 caracteres y contener al menos una letra mayúscula, una letra minúscula, un número y un carácter especial.' : '';
          break;
        case 'confirmPassword':
          this.formErrors.confirmPassword = !this.validateConfirmPassword() ? 'Las contraseñas no coinciden.' : '';
          break;
        case 'telefono':
          this.formErrors.telefono = !this.validateTelefono() ? 'El teléfono debe tener 10 dígitos (Incluyendo el indicativo de ciudad al inicio (ejm, 601, 604) seguido del numero de telefono), sin espacios.' : '';
          break;
        case 'celular':
          this.formErrors.celular = !this.validateCelular() ? 'Por favor, escoge el indicativo del país ingresa un número de celular válido.' : '';
          break;
      }
    },
    validateAndNext() {
      this.formSubmitted = true;
      this.validateField('email');
      this.validateField('password');
      this.validateField('confirmPassword');
      this.validateField('telefono');
      this.validateField('celular');

      if (!Object.values(this.formErrors).some(error => error)) {
        this.$emit('update', this.form);
        this.$emit('next');
      }
    },
    previousStep() {
      this.$emit('prev');
    }
  }
};
</script>

<style scoped>
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
.text-sm {
  font-size: 0.875rem;
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
.border-red-500 {
  border-color: #e53e3e;
}
</style>