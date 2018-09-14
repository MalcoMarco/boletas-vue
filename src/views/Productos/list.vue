<template>
    <div class="animated fadeIn">
        <b-card header-tag="header">
        <div slot="header">
            <i class="icon-drop"></i> Lista de Productos
        </div>
        <b-card-body>
            <div class="row">
                <b-col sm="12">
                    <b-button type="button" class="m-2 float-right" variant="primary"
                    @click="$router.push({name:'Crear Producto'})">Agregar</b-button> </b-col>
                <b-col sm="12" class="table-responsive">
                    <b-table striped hover :items="productos" :fields="fields">
                        <template slot="opciones" slot-scope="row">
                            <b-button size="sm" @click.stop="info(row.item, row.index, $event.target)" class="mr-1">
                            Editar
                            </b-button>
                        </template>
                    </b-table>
                </b-col>
            </div>
        </b-card-body>
        </b-card>
        <!-- modal edit producto -->
    <b-modal id="modalEditProducto" @hide="resetModal" @ok="EditProducto" title="Editar Producto" size="lg">
        <b-row class="justify-content-center">
            <b-col sm="12">
                <form @submit.stop.prevent="Edit_producto" name="formEditProducto">
                <b-card>
                    <b-row>
                        <b-col sm="12">
                        <b-form-group
                            label="Nombre"
                            label-for="name"
                            >
                            <b-form-input
                                v-model="editproducto.name" 
                                :state="editproducto.name.length>0" type="text" id="name" 
                                placeholder="Product name" required></b-form-input>
                        </b-form-group>
                        </b-col>
                    </b-row>
                    <b-row>
                        <b-col sm="12">
                        <b-form-group>
                            <label for="imagen">Url Imagen </label>
                            <b-form-input v-model="editproducto.imagen" type="url" id="imagen" placeholder="https://."></b-form-input>
                        </b-form-group>
                        </b-col>
                    </b-row>
                    <b-row>
                        <b-col sm="12">
                        <b-form-group>
                            <label for="ccnumber">Precio $</label>
                            <b-form-input v-model="editproducto.precio"
                            :state="editproducto.precio.length>0" 
                            type="number" id="ccnumber" required></b-form-input>
                        </b-form-group>
                        </b-col>
                    </b-row>
                    <b-row>
                        <b-col sm="12">
                        <b-form-group >
                            <label for="textarea1">Descripcion</label>
                            <b-form-textarea id="textarea1"
                                v-model="editproducto.descripcion"
                                placeholder="Enter something descripcion"
                                :rows="3"
                                :max-rows="6">
                            </b-form-textarea>
                        </b-form-group>
                        </b-col>
                    </b-row>
                </b-card>
                </form>
            </b-col>
        </b-row>
    </b-modal>
    </div>
</template>
<script>
export default {
  name: 'MisProductos',
  data () {
    return {
      productos:[],
      editproducto:{id:'',imagen:'',name:'',precio:'',descripcion:''},
      fields: ['id', 'name', 'descripcion', 'precio','updated_at', 'opciones'],
    }
  },
  created(){
      this.get_productos()
  },
  methods: {
    get_productos () {
        var url=process.env.VUE_APP_API_TEST+'productos';
        axios.get(url).then(response=>{
            this.productos=response.data.productos;
        }).catch(error=> {
            console.log(error)
        });
    },
    info (item, index, button) {
      this.editproducto.name =item.name// `Row index: ${index}`
      this.editproducto.descripcion =item.descripcion
      this.editproducto.imagen =item.imagen
      this.editproducto.precio =item.precio
      this.editproducto.id =item.id
      this.$root.$emit('bv::show::modal', 'modalEditProducto', button)
    },
    resetModal(){

    },
    Edit_Producto(){
        var url=process.env.VUE_APP_API_TEST+'productos/update';
        axios.post(url,this.editproducto).then(response=>{
            //this.productos=response.data.productos;
            toastr.info('Producto Actualizado')
            this.$root.$emit('bv::hide::modal','modalEditProducto')
            this.get_productos()
        }).catch(error=> {
            console.log(error)
        });
    },
    EditProducto(evt){
        // Prevent modal from closing
      evt.preventDefault()
      if (!this.editproducto.name || !this.editproducto.precio) {
        alert('Please Complete')
      } else {
        this.Edit_Producto()
      }
    }

  }
}
</script>
