<script>
import axios from 'axios';
export default {
  props: {
    modalTitle: {
      type: String,
      required: true,
    },
    btnAgregar: {
      type: Boolean,
      required: true,
    }
  },
    data() {
        return {
          btnActivo: true,
            formData: {
                nombres: '',
                apellidos: '',
                sexo: '',
                email: ''
            }
        };
    },
    watch:{
      btnAgregar: {
        immediate: true,
        handler(newVal){
          console.log(newVal)
          if(newVal == true){
            this.formData= {
                nombres: '',
                apellidos: '',
                sexo: '',
                email: ''
            }
          }
          this.btnActivo = newVal;
        }
      }
    },

    methods: {
      capturarBoton() {
            const formDataCopy = { ...this.formData };
            if(this.btnAgregar){
                  this.$emit("registroAgregar", formDataCopy); //guardar registro
                  this.$emit('closeModal'); //Cerrar modal
                  this.limpiarForm() //Liampiar el Formulario
            }else{
              this.$emit("registroActualizado", formDataCopy); //guardar registro
                  this.$emit('closeModal'); //Cerrar modal
                  this.limpiarForm() //Liampiar el Formulario

            }
        },

        limpiarForm(){
          this.formData.nombres = '',
          this.formData.apellidos = '',
          this.formData.sexo = '',
          this.formData.email = ''
          
        },
        EditForm(userData){
        this.formData = {...userData};
        this.isEditing = true
        
        }
    }
}

</script>
<template>

<!--<button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#exampleModal">
 Boton de Agregar
</button>-->

<!-- Modal -->
<div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h1 class="modal-title fs-5" id="exampleModalLabel">{{ modalTitle }}</h1>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <form @submit.prevent="capturarBoton">
      <div class="modal-body">
   
  <div class="mb-3">
    <label for="exampleInputEmail1" class="form-label">Nombres</label>
    <input type="text" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp" v-model="formData.nombres">
    
  </div>
  <div class="mb-3">
    <label for="exampleInputPassword1" class="form-label">Apellidos</label>
    <input type="text" class="form-control" id="exampleInputPassword1" v-model="formData.apellidos">
  </div>
 
  <div class="mb-3">
    <label for="exampleInputPassword1" class="form-label">Sexo</label>
    <input type="text" class="form-control" id="exampleInputPassword1" v-model="formData.sexo">
  </div>
  <div class="mb-3">
    <label for="exampleInputPassword1" class="form-label">Email</label>
    <input type="text" class="form-control" id="exampleInputPassword1" v-model="formData.email">
  </div>


      </div>

      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal" @submit.prevent="capturarBoton">Close</button>
      <!--  <button type="submit" class="btn btn-primary" @click="this.formData">Submit</button>-->
        <button type="submit" class="btn btn-primary"  v-if="btnAgregar" >Agregar</button>
        <button type="submit" class="btn btn-warning" v-else>Actualizar</button>
      </div>
    </form>
    </div>
  </div>
</div>
</template>

<style scoped></style>