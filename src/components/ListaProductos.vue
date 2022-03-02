<template>
  <div class="overflow-x-auto">
    <table class="table w-full table-zebra">
      <thead>
      <tr>
        <th></th>
        <th>Nombre</th>
        <th>Descripcion</th>
        <th>Precio</th>
        <th>Modelo</th>
        <th>Ver</th>
        <th></th>
      </tr>
      </thead>
      <tbody>
      <tr class="hover" v-for="(producto, i) in productos[0]" :key="i">
        <th>{{ producto.id }}</th>
        <td>{{ producto.nombre }}</td>
        <td>{{ producto.descripcion }}</td>
        <td>{{ producto.precio }}$</td>
        <td>{{ productos[1][i].nombre_modelo }}</td>
        <th>
<!--         Usamos este boton para traer un modal capaz de editar y ver la información-->
          <label for="ver-modal" class="btn btn-accent btn-xs" @click="fetchProducto(producto.id)">Detalles</label>
        </th>
        <th>
          <!--  Llamamos al evento de borrar en el inicio mediante el emitir-->
          <button class="btn btn-error btn-xs" @click="$emit('borrar-producto', producto.id)">Borrar</button>
        </th>
      </tr>
      </tbody>
    </table>
  </div>

  <input type="checkbox" id="ver-modal" class="modal-toggle">
  <div class="modal">
    <div class="modal-box">
      <div class="modal-action mt-0">
        <label for="ver-modal" class="btn btn-circle">X</label>
      </div>
      <div v-if="loaded === true">
        <!-- Cargamos la vista de los datos -->
        <VerProducto @editar-producto="editandoProducto" :producto="productoSelect" :image="image"></VerProducto>
      </div>
    </div>
  </div>
</template>

<script>
import VerProducto from "./VerProducto.vue";

export default {
  name: "ListaProductos",
  data() {
    return {
      productoSelect: [],
      loaded: false,
      image: ''
    }
  },
  props: {
    productos: Array,
  },
  components: {
    VerProducto
  },
  methods: {
    async fetchProducto(id) {
      const res = await fetch(`http://127.0.0.1:8000/api/productos/${id}`)
      this.productoSelect = await res.json();
      this.loaded = true;
      if (this.loaded) {
        // Aquí se realiza un slice para poder obtener la url de la img directamente del servidor
        const url = this.productoSelect[0].path.slice(7);
        this.image = "http://127.0.0.1:8000/storage/" + url
        console.log(this.image)
      }
    },
    editandoProducto(data) {
     // Emitimos el evento para actualizar los datos en la vista de inicio
      this.$emit('editado-producto', data)
    }
  },
  emits: ['borrar-producto', 'editado-producto']
}
</script>

<style scoped>

</style>
