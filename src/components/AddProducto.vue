<template>
  <div>
<!--    Toda la logica del formulario la manejamos en esta vista-->
    <h3 class="font-bold text-2xl">Añadir registro</h3>
    <p class="py-4">Crea un nuevo registro de producto</p>
    <form @submit="onSubmit" class="w-full">
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
          <option disabled selected>Selecciona modelo</option>
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
</template>

<script>
export default {
  name: "AddProducto",
  data() {
    return {
      // Creamos un modelo para trabajar los campos en el resquest
      form: {
        nombre: '',
        descripcion: '',
        precio: 0,
        cantidad: 0,
        imagen: null,
        nombre_modelo: '',
        descripcion_modelo: 'Descripcion del modelo predeterminada',
      }
    }
  },
  methods: {
    onChange(event) {
      // Asignamos la imagen al campo del formulario
      this.form.imagen = event.target.files
    },
    resetInput() {
      // Creamos este metodo para resetear la información luego de enviarla
      this.form.nombre = '';
      this.form.descripcion = '';
      this.form.precio = 0;
      this.form.cantidad = 0;
      this.form.imagen = null;
      document.getElementById("imagen").value = ""
      this.form.nombre_modelo = '';
      this.form.descripcion_modelo = 'Descripcion del modelo predeterminada';
    },
    async onSubmit(e) {
      // Este metodo crea un FormData que asigna los nombres de cada valor
      e.preventDefault()

      const formData = new FormData();
      formData.append('nombre', this.form.nombre);
      formData.append('descripcion', this.form.descripcion);
      formData.append('precio', this.form.precio);
      formData.append('cantidad', this.form.cantidad);
      formData.append('imagen', this.form.imagen[0]);
      formData.append('nombre_modelo', this.form.nombre_modelo);
      formData.append('descripcion_modelo', this.form.descripcion_modelo);

      // Mandamos el request a nuestra API
      const res = await fetch("http://127.0.0.1:8000/api/productos/",
          {
            body: formData,
            method: "post"
          })
      const data = await res.json()
      // Emitimos el evento que nos actualizara la vista en el inicio
      this.$emit('add-producto', data)
      this.resetInput()


    }
  }
}
</script>

<style scoped>

</style>
