<template>
  <div class="fixed inset-0 bg-gray-500 bg-opacity-75 flex items-center justify-center">
    <div class="bg-white p-8 rounded-lg shadow-lg relative max-w-md w-full mx-4">
      <slot></slot>
      <button @click="$emit('close')" class="absolute top-2 right-2 bg-red-500 text-white p-2 rounded-full hover:bg-red-600 transition duration-200">Cerrar</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ModalComponente',
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
.bg-red-500 {
  background-color: #f56565;
}
.text-white {
  color: #fff;
}
.hover\:bg-red-600:hover {
  background-color: #e53e3e;
}
.transition {
  transition-property: all;
}
.duration-200 {
  transition-duration: 200ms;
}
.absolute {
  position: absolute;
}
.top-2 {
  top: 0.5rem;
}
.right-2 {
  right: 0.5rem;
}
.p-2 {
  padding: 0.5rem;
}
.rounded-full {
  border-radius: 9999px;
}
</style>