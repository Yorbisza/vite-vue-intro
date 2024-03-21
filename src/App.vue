<script>
import axios from 'axios';
import Swal from 'sweetalert2';
import add from './components/Agregar.vue';


export default {
  components: {
    add
  },
  data() {
    return {

      modalTitle: 'Agregar Usuario',
      btnAgregar: true,
      ListaUsuario: []
      // Define ListaUsuario dentro de data
    };
  },
  mounted() {
    this.Lista();
  },
  methods: {
    Lista() {
      // Hacemos una petición a la API v1 de Nube Colectiva
      axios.get('https://65f34f70105614e654a05799.mockapi.io/inea/api/v1/user') // Agrega https:// antes de la URL
        .then((response) => {
          // Asignamos los datos obtenidos a ListaUsuario
          this.ListaUsuario = response.data;
        })
        .catch((error) => {
          // Si hubo algún error, lo mostramos
          console.log(error);
        })
        .finally(() => {
          // Se ejecutó sin problemas
          console.info('Finalizó');
        });
    },
    abrirModal(tipo, userData) {
      const myModal = new bootstrap.Modal(document.getElementById('exampleModal'));
      if (tipo === 'agregar') {
        
        this.modalTitle = 'Agregar Registro';
        this.btnAgregar = true;
        
      } else if (tipo === 'actualizar') {
        this.modalTitle = 'Actualizar Registro';
        this.btnAgregar = false;
        this.$refs.modal.EditForm(userData);
        // this.$refs.modal.EditForm(userData);
      }
      myModal.show();

    },
    closeModal() {
      if (this.$refs.modal && this.$refs.modal.$el) {

        this.$refs.modal.$el.style.display = 'none';

        const modalBackdrop = document.querySelector('.modal-backdrop');
        if (modalBackdrop) {
          modalbackdrop.remove();
        }
      }

    },

    async agregarRegistro(registroAgregar) {
      await axios.post('https://65f34f70105614e654a05799.mockapi.io/inea/api/v1/user', registroAgregar) // Agrega https:// antes de la URL
        .then((response) => {
          this.ListaUsuario = []
          Swal.fire({
            position: "top-end",
            icon: "success",
            title: "Creado satisfactoriamente!",
            showConfirmButton: false,
            timer: 1500
          })
          this.Lista();
        })

        .catch(error => {

          Swal.fire({
            position: "top-end",
            icon: "error",
            title: "Error",
            showConfirmButton: false,
            timer: 1500
          });
        })
    },
    async actualizarRegistro(registroActualizado) {
     // await axios.put(`https://65f34f70105614e654a05799.mockapi.io/inea/api/v1/user/${registroActualizado.id}`) // Agrega https:// antes de la URL
      await axios.put('https://65f34f70105614e654a05799.mockapi.io/inea/api/v1/user/' + registroActualizado.id, registroActualizado) // Agrega https:// antes de la URL
        .then((response) => {
          this.ListaUsuario = []
          Swal.fire({
            position: "top-end",
            icon: "success",
            title: "Actualizado satisfactoriamente!",
            showConfirmButton: false,
            timer: 1500
          })
          this.Lista();
        })

        .catch(error => {

          Swal.fire({
            position: "top-end",
            icon: "error",
            title: "Error",
            showConfirmButton: false,
            timer: 1500
          });
        })
    },
   /* async actualizarRegistro(registroAgregar) {
    await axios.put(`https://65f34f70105614e654a05799.mockapi.io/inea/api/v1/user/${registroActualizado}`, registroAgregar) // Agrega https:// antes de la URL
        .then((response) => {
          this.ListaUsuario = []
          Swal.fire({
            position: "top-end",
            icon: "success",
            title: "Creado satisfactoriamente!",
            showConfirmButton: false,
            timer: 1500
          })
          this.Lista();
        })

        .catch(error => {

          Swal.fire({
            position: "top-end",
            icon: "error",
            title: "Error",
            showConfirmButton: false,
            timer: 1500
          });
        })
    },*/


    Delete(row) {
      Swal.fire({
        title: "Are you sure?",
        text: `You won't be able to revert this!${row.nombres}`,
        icon: "warning",
        showCancelButton: true,
        confirmButtonColor: "#3085d6",
        cancelButtonColor: "#d33",
        confirmButtonText: "Yes, delete it!"
      }).then((result) => {
        if (result.isConfirmed) {
          axios.delete(`https://65f34f70105614e654a05799.mockapi.io/inea/api/v1/user/${row.id}`) // Agrega https:// antes de la URL
            .then((response) => {
              this.ListaUsuario = []
              Swal.fire({
                position: "top-end",
                icon: "success",
                title: "Your work has been saved",
                showConfirmButton: false,
                timer: 1500
              });
              this.Lista()
            })
            .catch((error) => {
              // Si hubo algún error, lo mostramos
              Swal.fire({
                position: "top-end",
                icon: "error",
                title: "Error",
                showConfirmButton: false,
                timer: 1500
              });
            })
            .finally(() => {
              // Se ejecutó sin problemas
            });
        }
      });
    }
  }
}


</script>

<template>
  <add ref="modal" 
  :modalTitle="modalTitle"
   :btnAgregar="btnAgregar"
   @registroAgregar="agregarRegistro"
    @closeModal="closeModal"
   @registroActualizado="actualizarRegistro">

  </add>

  <div class="container">
    <h1> LISTA DE USUARIOS</h1>
    <div class="btn-group" role="group">
      <button type="button" class="btn btn-success" @click="abrirModal('agregar', 'data')">
        Agregar
      </button>
    </div>
    <hr>
  </div>


  <table class="table">
    <thead>
      <tr>
        <th scope="col">#</th>
        <th scope="col">Nombre</th>
        <th scope="col">Apellidos</th>
        <th scope="col">Sexo</th>
        <th scope="col">Email</th>
        <th scope="col">Accion</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(user, index) in ListaUsuario" :key="index">
        <td scope="row">{{ user.id }}</td>
        <th>{{ user.nombres.toUpperCase() }}</th>
        <td>{{ user.apellidos }}</td>
        <td>{{ user.sexo }}</td>
        <td>{{ user.email }}</td>
        <td>
          <div class="btn-group" role="group">
            <button class="btn btn-sm btn-warning" @click="abrirModal('actualizar', user)">Editar</button>
            <button class="btn btn-sm btn-danger" @click="Delete(user)">Eliminar</button>
          </div>
        </td>
      </tr>
    </tbody>
  </table>

</template>

<style scoped></style>