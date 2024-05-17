<template>
  <div class="principio" style="text-align: center">
   


    <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#productModal">
      Abrir Formulario
    </button>

    
    <div class="modal fade" id="productModal" tabindex="-1" aria-labelledby="productModalLabel" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="productModalLabel">Formulario</h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            <span v-if="errornombre" class="error">{{ errornombre }}</span>
            <label for="name">Nombre del producto</label>
            <input type="text" v-model="info" class="form-control" /><br />
            <span v-if="errorprecio" class="error">{{ errorprecio }}</span>
            <label for="price">Digite el precio</label>
            <input type="text" v-model="ap" class="form-control" /><br />
            <span v-if="errorcosto" class="error">{{ errorcosto }}</span>
            <label for="cost">Digite el costo</label>
            <input type="text" v-model="co" class="form-control" /><br />
            <span v-if="errorproveedor" class="error">{{ errorproveedor }}</span>
            <label for="provider">Digite el nombre del proveedor</label>
            <input type="text" v-model="fe" class="form-control" /><br />
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cerrar</button>
            <button type="button" class="btn btn-primary" @click="guardar">Agregar</button>
          </div>
        </div>
      </div>
    </div>

    <table class="tabla">
      <thead>
        <tr>
          <th>Nombre</th>
          <th>Precio</th>
          <th>Costo</th>
          <th>Cantidad</th>
          <th>Proveedor</th>
          <th>A</th>
          <th>D</th>
          <th>Ganancias</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in data" :key="index">
          <td>{{ item.nombre }}</td>
          <td>{{ item.precio }}</td>
          <td>{{ item.costo }}</td>
          <td>{{ item.cantidad }}</td>
          <td>{{ item.proveedor }}</td>
          <td>
            <button @click="aumentar(index)" v-if="item.cantidad < maxc">+</button>
          </td>
          <td>
            <button @click="disminuir(index)" v-if="item.cantidad > 0">-</button>
          </td>
          <td>{{ ganancia(item) }}</td>
        </tr>
      </tbody>
    </table>

    <div class="cosas">
      <p>Costo total del inventario: {{ costoTotal() }}</p>
      <p>Precio total del inventario: {{ precioTotal() }}</p>
      <p>Ganancia total: {{ gananciaTotal() }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

let info = ref("");
let ap = ref("");
let co = ref("");
let fe = ref("");
let data = ref([]);
const maxc = 100;
let errornombre = ref("");
let errorprecio = ref("");
let errorcosto = ref("");
let errorproveedor = ref("");

function guardar() {
  let valid = true;
  if (!info.value) {
    errornombre.value = "Por favor, ingresa el Nombre.";
    setTimeout(() => {
      errornombre.value = "";
    }, 3000);
    valid = false;
  }

  if (!ap.value || isNaN(parseFloat(ap.value))) {
    errorprecio.value = "Por favor, ingresa un Precio válido.";
    setTimeout(() => {
      errorprecio.value = "";
    }, 3000);
    valid = false;
  }

  if (!co.value || isNaN(parseFloat(co.value))) {
    errorcosto.value = "Por favor, ingresa un Costo válido.";
    setTimeout(() => {
      errorcosto.value = "";
    }, 3000);
    valid = false;
  }

  if (!fe.value) {
    errorproveedor.value = "Por favor, ingresa el Proveedor.";
    setTimeout(() => {
      errorproveedor.value = "";
    }, 3000);
    valid = false;
  }

  if (valid) {
    let d = {
      nombre: info.value,
      precio: parseFloat(ap.value),
      costo: parseFloat(co.value),
      proveedor: fe.value,
      cantidad: 0
    };
    data.value.push(d);
    info.value = "";
    ap.value = "";
    co.value = "";
    fe.value = "";
  }
}

function aumentar(index) {
  if (data.value[index].cantidad < maxc) {
    data.value[index].cantidad++;
  }
}

function disminuir(index) {
  if (data.value[index].cantidad > 0) {
    data.value[index].cantidad--;
  }
}

function ganancia(item) {
  return (item.precio - item.costo) * item.cantidad;
}

function costoTotal() {
  let total = 0;
  data.value.forEach((item) => {
    total += item.costo * item.cantidad;
  });
  return total;
}

function precioTotal() {
  let total = 0;
  data.value.forEach((item) => {
    total += item.precio * item.cantidad;
  });
  return total;
}

function gananciaTotal() {
  return precioTotal() - costoTotal();
}
</script>

<style >



.tabla{
  width: 10%;
  right: 500px;
  box-shadow: 0 0 20px #6260da;
  background-color: white;
  margin-top: 20%;
  margin-left: 50px;
  
}
th,td{
  width: 25%;
  text-align: left;
  vertical-align: top;
  border: 1px solid #000000;
  border-spacing: 0;
  padding: 0.3em;
 
}

.error{
  color: red;
}

.btn {
  margin-top: 30%;
  
}

body{
  background-color: aqua;
}
.principio {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  margin-top: 20px;
}

.cosas{
  margin-top: 10%;
  
}

</style>
