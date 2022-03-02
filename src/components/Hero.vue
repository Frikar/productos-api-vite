<template>
  <div class="hero min-h-16 bg-base-100">
    <div class="text-center hero-content p-32 pb-10">
      <div class="max-w-xl">
        <h1 class="text-6xl font-bold">Productos Demo</h1>
        <p class="py-6">Este proyecto es una demo usando Laravel y Vue para crear un sistema de inventario para una tienda de laptops. Para desarrollarlo se usó Laravel 8, Vue 3, Vite y Tailwind + DaisyUI.</p>
<!--        El boton de añadir registros nos permite trabajar con un modal para añadir datos-->
        <label for="modal-add" class="btn btn-primary">Añadir registro</label>
      </div>
    </div>
  </div>
  <input type="checkbox" id="modal-add" class="modal-toggle">
  <div class="modal" >
    <div class="modal-box max-w-xl">
      <div class="modal-action mt-0">
        <label for="modal-add" class="btn btn-circle">X</label>
      </div>
<!--      Creamos un componente que lleve la logica de agregar-->
      <AddProducto @add-producto="mostrandoProducto"></AddProducto>
    </div>
  </div>
</template>

<script>
import AddProducto from "./AddProducto.vue";

export default {
  name: "Hero",
  props: {
    data: Object
  },
  components: {
    AddProducto
  },
  methods: {
    // Hacemos el evento y su metodo para enviar la data al componente superior
    mostrandoProducto(data) {
      this.$emit('mostrar-producto', data)
    }
  },
  emits: ['mostrar-producto']
}
</script>

<style scoped>

</style>
