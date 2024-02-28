<template>
  <div>
     <section class="register" id="register">
       <h2 class="error" v-if="error != ''">{{ error }}</h2>
       <h4>Ingrese su Nombre</h4>
       <input type="text" class="controls" placeholder="Nombre..." v-model.trim="nombre" />
       <h4>Ingrese sus Apellidos</h4>
       <input type="text" class="controls" placeholder="Apellidos..." v-model.trim="apellido" validation="required" />
       <h4>Tipo de Documento</h4>
       <select name="tipodocumento" class="selects" v-model="tipodocumento">
         <option value="TI">Tarjeta de identidad</option>
         <option value="CC">Cedula de ciudadania</option>
         <option value="CE">Cedula de extranjeria</option>
       </select>
       <h4>Ingrese el numero del Documento</h4>
       <input type="number" class="controls" placeholder="Numero de documento" v-model.trim="numdocumento"
         validation="required" />
       <h4>Seleccione su Genero</h4>
       <select name="genero" class="selects" v-model="genero">
         <option value="man">Masculino</option>
         <option value="women">Femenino</option>
       </select>
       <h4>Ingrese su correo</h4>
       <input type="text" class="controls" placeholder="correo..." v-model.trim="correo" validation="required" />
       <h4>Ingrese su telefono</h4>
       <input type="number" class="controls" placeholder="Telefono..." v-model.trim="telefono" validation="required" />
       <h4>Ingrese su fecha de Nacimiento</h4>
       <input type="date" class="controls" placeholder="Fecha..." v-model.trim="date" validation="required" />
       <button class="Registrar" @click="validar()">Enviar Registro</button>
     </section>
     <table>
       <thead>
         <tr>
           <th>Nombre</th>
           <th>Apellido</th>
           <th>Tipo de documento</th>
           <th>Numero de documento</th>
           <th>Genero</th>
           <th>Correo</th>
           <th>Telefono</th>
           <th>Fecha de nacimiento</th>
           <th>Opciones</th>
         </tr>
       </thead>
       <tbody>
         <tr v-for="(item, i) in registros" :key="i" :style="item.edad > 17 ? 'color:pink' : 'color:purple'">
           <td>{{ item.nombre }}</td>
           <td>{{ item.apellido }}</td>
           <td>{{ item.tipodocumento }}</td>
           <td>{{ item.numdocumento }}</td>
           <td>{{ item.genero }}</td>
           <td>{{ item.correo }}</td>
           <td>{{ item.telefono }}</td>
           <td>{{ item.date }}</td>
           <td>
             <button @click="eliminar(i)">❌</button>
             <button @click="editar(item, i)">📝</button>
           </td>
         </tr>
 
       </tbody>
 
     </table>
 
   </div>
 </template>
 
 <script setup>
 import { ref } from "vue";
 let nombre = ref("");
 let apellido = ref("");
 let tipodocumento = ref("");
 let numdocumento = ref("");
 let genero = ref("");
 let correo = ref("");
 let telefono = ref("");
 let date = ref("");
 let error = ref("");
 let registros = ref([]);
 let bd = true;
 let index = null
 
 
 function validar() {
   if (bd == true) {
     if (nombre.value === "") {
       error.value = "Nombre no debe estar vacío";
       setTimeout(() => {
         error.value = "";
       }, 3000);
     } else if (apellido.value === "") {
       error.value = "Apellido no debe estar vacío";
       setTimeout(() => {
         error.value = "";
       }, 3000);
     } else if (tipodocumento.value === "") {
       error.value = "Tipo de documento no debe estar vacío";
       setTimeout(() => {
         error.value = "";
       }, 3000);
     } else if (numdocumento.value === "") {
       error.value = "El número de documento no debe estar vacío";
       setTimeout(() => {
         error.value = "";
       }, 3000);
     } else if (genero.value === "") {
       error.value = "Género no debe estar vacío";
       setTimeout(() => {
         error.value = "";
       }, 3000);
     } else if (!/^\S+@\S+\.\S+$/.test(correo.value)) {
       error.value = "Correo no es válido";
       setTimeout(() => {
         error.value = "";
       }, 3000);
     } else if (telefono.value === "") {
       error.value = "Teléfono no debe estar vacío";
       setTimeout(() => {
         error.value = "";
       }, 3000);
     } else if (date.value === "") {
       error.value = "Fecha de nacimiento no debe estar vacía";
       setTimeout(() => {
         error.value = "";
       }, 3000);
     } else {
       const fechaNacimiento = new Date(date.value);
       const hoy = new Date();
       if (fechaNacimiento > hoy) {
         error.value = "La fecha de nacimiento no puede ser en el futuro";
         setTimeout(() => {
           error.value = "";
         }, 3000);
       } else {
         Registrar();
       }
     }
 
   } else {
 
     registros.value[index].nombre = nombre.value
     registros.value[index].apellido = apellido.value
     registros.value[index].tipodocumento = tipodocumento.value
     registros.value[index].numdocumento = numdocumento.value
     registros.value[index].genero = genero.value
     registros.value[index].correo = correo.value
     registros.value[index].telefono = telefono.value
     registros.value[index].date = date.value
     bd = true
     limpiar()
   }
 
 }
 
 function Registrar() {
   const Registros = {
     nombre: nombre.value,
     apellido: apellido.value,
     tipodocumento: tipodocumento.value,
     numdocumento: numdocumento.value,
     genero: genero.value,
     correo: correo.value,
     telefono: telefono.value,
     date: date.value,
   };
   registros.value.push(Registros);
   console.log(registros);
   limpiar();
 }
 
 function eliminar(i) {
   registros.value.splice(i, 1)
 }
 
 function editar(item, i) {
   console.log(item);
   console.log(i);
   nombre.value = item.nombre
   apellido.value = item.apellido
   tipodocumento.value = item.tipodocumento
   numdocumento.value = item.numdocumento
   genero.value = item.genero
   correo.value = item.correo
   telefono.value = item.telefono
   date.value = item.date
   index = i
   bd = false
 }
 
 function limpiar() {
   nombre.value = "";
   apellido.value = "";
   tipodocumento.value = "";
   numdocumento.value = "";
   genero.value = "";
   correo.value = "";
   telefono.value = "";
   date.value = "";
 }
 </script>
 
 <style scoped>
 body{
   background-color: #000;
 }
 .register {
   width: 100%;
   background: #24303c;
   padding: 30px;
   margin: auto;
   margin-top: 30px;
   margin-bottom: 30px;
   border-radius: 4px;
   font-family: 'calibri';
   color: white;
   box-shadow: 7px 13px 37px #000;
 }
 
 .controls {
   width: 90%;
   background: #24303c;
   padding: 10px;
   border-radius: 4px;
   border: 1px solid #1f53c5;
   font-family: 'calibri';
   font-size: 18px;
   color: white;
 }
 
 .h4 {
   margin: 2px;
 }
 
 th,
 tr,
 td {
   border: 2px solid rgb(96, 96, 192);
   color: white;
 }
 
 .selects {
   width: 96%;
   background: #24303c;
   border: 1px solid #1f53c5;
   border-radius: 4px;
   padding: 10px;
   color: white;
   font-size: 16px;
 }
 
 .Registrar {
   background-color: #0e0e0f;
   margin-top: 12px;
   cursor: pointer;
   color: white;
 }
 </style>
 