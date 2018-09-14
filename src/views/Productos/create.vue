<template>
    <div class="animated fadeIn">
        <b-card header-tag="header">
        <div slot="header">
            <i class="icon-drop"></i> Agregar Productos
        </div>
        <b-card-body>
            <b-row class="justify-content-center">
                <b-col sm="8">
                    <form @submit.prevent="crear_producto">
                    <b-card>
                        <div slot="header">
                            <strong>Nuevo Producto </strong> <small>Form</small>
                        </div>
                        <b-row>
                            <b-col sm="12">
                            <b-form-group>
                                <label for="name">Nombre </label>
                                <b-form-input v-model="producto.name" type="text" id="name" placeholder="Product name" required></b-form-input>
                            </b-form-group>
                            </b-col>
                        </b-row>
                        <b-row>
                            <b-col sm="12">
                            <b-form-group>
                                <label for="ccnumber">Precio $</label>
                                <b-form-input v-model="producto.precio" type="number" id="ccnumber" required></b-form-input>
                            </b-form-group>
                            </b-col>
                        </b-row>
                        <b-row>
                            <b-col sm="12">
                            <b-form-group >
                                <label for="textarea1">Descripcion</label>
                                <b-form-textarea id="textarea1"
                                    v-model="producto.descripcion"
                                    placeholder="Enter something descripcion"
                                    :rows="3"
                                    :max-rows="6">
                                </b-form-textarea>
                            </b-form-group>
                            </b-col>
                        </b-row>
                        <div class="form-actions">
                            <b-button type="submit" class="m-2" variant="primary">Guardar</b-button>
                            <b-button type="button" @click="resetProducto()"  class="m-2" variant="secondary">Cancel</b-button>
                        </div>
                    </b-card>
                    </form>
                </b-col>
            </b-row>
        </b-card-body>
        </b-card>
    </div>
</template>
<script>
export default {
  name: 'productocreate',
  data () {
    return {
      producto:{name:'',precio:0,descripcion:''}
    }
  },
  methods: {
    crear_producto () {
      var url=process.env.VUE_APP_API+'productos/store';
            axios.post(url,this.producto).then(response=>{
                this.resetProducto();
                toastr.success('Producto Agreagado Correctamente')
                console.log(response)
            }).catch(error=> {
                console.log(error)
            });
    },
    resetProducto(){
        this.producto.name=""
        this.producto.precio=0
        this.producto.descripcion=""
        //toastr.error('RESET');
    }
  }
}
</script>
