<template>

  <h3 class="font-bold text-2xl">{{ editable === true ? 'Edita el producto' : 'Ver el producto' }}</h3>
  <p class="py-4">Información y datos:</p>
  <!-- Usamos este v-if para mostrar la info o mostrar los formularios de edición -->
  <div v-if="editable === false">
    <div class="avatar">
      <div class="w-48 rounded-full">
        <img :src='image' alt="Imagen">
      </div>
    </div>
    <p>{{ this.producto[0].nombre }}</p>
    <p>{{ this.producto[0].descripcion }}</p>
    <p>{{ this.producto[0].precio }}$</p>
    <p>{{ this.producto[0].cantidad }} unidades</p>
    <p>{{ this.producto[1].nombre_modelo }}</p>
    <p>{{ this.producto[1].descripcion_modelo }}</p>
  </div>
  <div v-if="editable">
    <!-- Formulario de edición usando la misma base que el formulario de creación -->
    <form class="w-full" @submit="onSubmit">
      <div class="form-control items-center">
        <label for="nombre" class="label">
          <span class="label-text font-bold">Nombre del producto</span>
        </label>
        <input id="nombre" type="text" minlength="10" maxlength="200" required
               class="input input-bordered w-full max-w-xs" v-model="form.nombre">
      </div>

      <div class="form-control items-center">
        <label for="descripcion" class="label">
          <span class="label-text font-bold">Descripción del producto</span>
        </label>
        <textarea id="descripcion" class="textarea textarea-bordered w-full max-w-xs"
                  v-model="form.descripcion" required minlength="20"></textarea>
      </div>

      <div class="form-control items-center">
        <label for="precio" class="label">
          <span class="label-text font-bold">Precio del producto</span>
        </label>
        <input id="precio" type="number" min="1" max="5000" required class="input input-bordered w-full max-w-xs"
               v-model="form.precio">
      </div>

      <div class="form-control items-center">
        <label for="cantidad" class="label">
          <span class="label-text font-bold">Cantidad</span>
        </label>
        <input id="cantidad" required type="number" min="1" max="10" class="input input-bordered w-full max-w-xs"
               v-model="form.cantidad">
      </div>

      <div class="form-control items-center">
        <label for="imagen" class="label">
          <span class="label-text font-bold">Imagen</span>
        </label>
        <input id="imagen" required type="file" class="w-full max-w-xs" alt="" @change="onChange">
      </div>

      <div class="form-control items-center">
        <label for="nombre_modelo" class="label">
          <span class="label-text font-bold">Modelos</span>
        </label>
        <select required id="nombre_modelo" class="select w-full max-w-xs select-bordered" v-model="form.nombre_modelo">
          <option disabled value="" selected>Selecciona modelo</option>
          <option>ASUS X455L</option>
          <option>Legion Lenovo Y50</option>
          <option>HP Omen</option>
          <option>ASUS X555L</option>
        </select>
      </div>

      <div class="form-control items-center mt-4">
        <input type="submit" value="Guardar" class="btn-success btn-md btn"/>
      </div>
    </form>
  </div>
  <div class="mt-4">
    <!-- Boton para cambiar entre la edicion o la muestra de los datos -->
    <button class="btn-error btn-md btn" @click="toggle">{{ editable === true ? 'Cancelar' : 'Editar' }}
    </button>
  </div>
</template>

<script>
export default {
  name: "VerProducto",
  data() {
    return {
      editable: false,
      // Creamos un modelo para añadir la información existente
      form: {
        nombre: this.producto[0].nombre,
        descripcion: this.producto[0].descripcion,
        precio: this.producto[0].precio,
        cantidad: this.producto[0].cantidad,
        imagen: null,
        nombre_modelo: this.producto[1].nombre_modelo,
        descripcion_modelo: this.producto[1].descripcion_modelo,
      }
    }
  },
  props: {
    producto: Array,
    image: String,
  },
  methods: {
    onChange(event) {
      // Asignamos la imagen al campo del formulario
      this.form.imagen = event.target.files
    },
    async onSubmit(e) {
      // Este metodo edita la información con un FormData
      e.preventDefault()

      const formData = new FormData();
      formData.append('nombre', this.form.nombre);
      formData.append('descripcion', this.form.descripcion);
      formData.append('precio', this.form.precio);
      formData.append('cantidad', this.form.cantidad);
      formData.append('imagen', this.form.imagen[0]);
      formData.append('nombre_modelo', this.form.nombre_modelo);
      formData.append('descripcion_modelo', this.form.descripcion_modelo);
      formData.append('_method', 'PUT');

      const res = await fetch(`http://127.0.0.1:8000/api/productos/${this.producto[0].id}`,
          {
            body: formData,
            method: "post"
          })
      const data = await res.json()
      // Enviamos al componente padre la información
      this.$emit('editar-producto', data)
      this.editable = !this.editable;
      console.log(data)
    },
    toggle() {
      // Activamos la edición mediante este booleano
      this.editable = !this.editable;
    }
  },
  emits: ['editar-producto']
}
</script>

<style scoped>

</style>
