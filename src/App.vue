<template>
  <nav class="sticky top-0 z-40">
    <div class="navbar bg-primary text-primary-content">
      <a class="normal-case text-2xl text-white font-bold">Productos</a>
    </div>
  </nav>
  <Hero @mostrar-producto="nuevoProducto"/>
<!--  Enviamos y recibimos desde la lista de produdctos los valores necesarios para actualizar los objetos-->
  <ListaProductos @borrar-producto="borrarProducto" @editado-producto="editadoProducto" :productos="productos"/>
</template>

<script>
import Hero from "./components/Hero.vue";
import ListaProductos from "./components/ListaProductos.vue";

export default {
  name: "App",
  components: {
    Hero,
    ListaProductos
  },
  data() {
    return {
      productos: [],
    }
  },
  methods: {
    nuevoProducto(data) {
      // Pusheamos a la lista de productos los nuevos traidos desde el componente AddProducto
      this.productos[0] = [...this.productos[0], data[0]]
      this.productos[1] = [...this.productos[1], data[1]]
    },
    editadoProducto(data) {
      // Aqui mapeamos entre todos los datos disponibles y asignamos los valores a los items correspondientes
      this.productos[0] = this.productos[0].map((producto) => producto.id === data[0].id ?
          {
            ...producto,
            nombre: data[0].nombre,
            descripcion: data[0].descripcion,
            precio: data[0].precio,
            cantidad: data[0].cantidad,
            filename: data[0].filename,
            path: data[0].path,
          } : producto)

      this.productos[1] = this.productos[1].map((producto) => producto.producto_id === data[0].id ?
          {
            ...producto,
            nombre_modelo: data[1].nombre_modelo,
            descripcion_modelo: data[1].descripcion_modelo,
          } : producto)
    },
    async fetchProductos() {
      const res = await fetch('http://127.0.0.1:8000/api/productos/')
      return res.json()
    },
    async borrarProducto(id) {
      //En este metodo buscamos los objectos que tengan un id para poder realizar el DELETE desde la API
      if (confirm('¿Estás seguro?')) {
        const res = await fetch(`http://127.0.0.1:8000/api/productos/${id}`, {
          method: 'DELETE'
        })
        res.status === 200 ? (this.productos[0] = this.productos[0].filter((producto) => producto.id !== id)) : alert('Error al borrar producto')
        res.status === 200 ? (this.productos[1] = this.productos[1].filter((producto) => producto.producto_id !== id)) : alert('Error al borrar modelo')
      }
    }
  },
  //Traemos los registros al crear el componente
  async created() {
    this.productos = await this.fetchProductos()
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
</style>
