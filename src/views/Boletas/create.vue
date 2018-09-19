<template>
    <div class="animated fadeIn">
        <b-card header-tag="header">
        <div slot="header">
            <i class="icon-drop"></i> Nueva Boleta
        </div>
        <b-card-body>
        <form @submit.prevent="form_newboleta">
            <div class="row">
                <b-col sm="12">
                    <h3 class="text-center">Boleta de Compra <br> Sitaav AulaPeru-TIC</h3>
                </b-col>
            </div>
            <b-row>
                <b-col sm="3" class="offset-9">
                    <b-form-group>
                        <b-form-input v-model="boleta.fecha"  type="date" id="name" required></b-form-input>
                    </b-form-group>
                </b-col>
                <b-col sm="12">
                    <b-form-group id="fieldsetHorizontal1"
                                    horizontal
                                    :label-cols="2"
                                    breakpoint="md"
                                    :description="dniDesc"
                                    label="Documento de Identidad:"
                                    label-for="dni">
                                     <b-input-group>
                        <b-form-input v-model="boleta.dni" id="dni" required></b-form-input>
                        <b-input-group-append>
                            <b-btn variant="primary" @click.prevent="buscar_dni"><i class="fa fa-search"></i></b-btn>
                        </b-input-group-append>
                        </b-input-group>
                    </b-form-group>
                </b-col>
                <b-col sm="12">
                    <b-row>
                        <b-col sm="6">
                            <b-form-group id="fieldsetHorizontal2"
                                    horizontal
                                    :label-cols="4"
                                    breakpoint="md"
                                    label="Señor(es):"
                                    label-for="name">
                                <b-form-input v-model="boleta.nombres" id="name"  placeholder="Nombres" required></b-form-input>
                            </b-form-group>
                        </b-col>
                        <b-col sm="3">
                            <b-form-input v-model="boleta.apepaterno" id="Paterno" placeholder="A.Paterno"></b-form-input>
                        </b-col>
                        <b-col sm="3">
                            <b-form-input v-model="boleta.apematerno" id="Materno"  placeholder="A.Materno"></b-form-input>
                        </b-col>
                    </b-row>
                </b-col>
                <b-col sm="12">
                    <b-form-group id="fieldsetHorizontal3"
                                    horizontal
                                    :label-cols="2"
                                    breakpoint="md"                                    
                                    label="Direccion:"
                                    label-for="Direccion">
                        <b-form-input v-model="boleta.direccion" id="Direccion"></b-form-input>
                    </b-form-group>
                </b-col>
            </b-row>
            <b-row class="justify-content-center">
                <b-col sm="12">
                    <b-button @click="showModal" variant="success" class="float-right m-2">Elegir Productos</b-button> 
                    <!-- the modal elegir productos -->
                    <b-modal id="myModal" ref="myModalRef" @ok="okModalProductos" size="lg"  title="Productos">
                    <b-table striped responsive :items="productos" :fields="fields">
                        <template responsive slot="opciones" slot-scope="row">
                            <input @click.stop type="checkbox"
                                @change="addI(row.item,checkedItems[row.item.id])"
                                v-model="checkedItems[row.item.id]">
                                {{checkedItems[row.item.id]==true?'Agregado':'' }}
                        </template>
                        <template slot="row-details" slot-scope="row">
                        </template>
                    </b-table>
                    </b-modal>      
                </b-col>             
                <b-col sm="12">
                    <b-card class="table-responsive"><!-- Tabla de boletaProductos -->
                        <table class="table bordered">
                        <tr>
                            <th>Cantidad</th><th>Producto</th><th>P.Unitario</th><th>Importe</th><th>Observacion</th>
                        </tr>
                        <tr v-for="prod in boleta.productos" :key="prod.id">
                            <td><input v-model="prod.cantidad" type="number" min="0" class="form-control" style="width:80px"></td>
                            <td v-text="prod.name"></td>
                            <td v-text="prod.precio"></td>
                            <td>S/<b v-text="prod.importe=prod.cantidad*prod.precio"></b> </td>
                            <td><b-form-textarea
                                    v-model="prod.descripcion"
                                    placeholder="something descripcion"
                                    :rows="1"
                                    :max-rows="3">
                                </b-form-textarea>
                            </td>
                        </tr>                                              
                        <tr><td colspan="5"></td></tr>
                        <tr>
                            <td colspan="3"></td>
                            <td><b>Descuento</b></td>
                            <td>
                                <b-input-group>
                                    <b-input-group-prepend>
                                    <b-input-group-text>S/</b-input-group-text>
                                    </b-input-group-prepend>
                                    <b-form-input v-model="boleta.descuento" id="desc" type="number" min="0" style="width:90px" required></b-form-input>                                    
                                </b-input-group>
                            </td>
                        </tr>
                        <tr>
                            <td colspan="3"></td>
                            <td><b>TOTAL</b></td>
                            <td><b v-text="'S/'+pagototal"></b></td>
                        </tr>
                    </table>
                    <div slot="footer">
                        <p class="text-center">
                            <b-button type="submit" variant="primary" class="m-2">Guardar</b-button>
                            <b-button variant="secondary" class="m-2">Cancelar</b-button>
                        </p>
                    </div>
                </b-card>
                </b-col>
            </b-row>
        </form>
        </b-card-body>
        </b-card>
    </div>
</template>
<script>
export default {
  name: 'BoletaCreate',
    data () {
        return {
        boleta:{dni:'',apepaterno:'',apematerno:'',nombres:'',fecha:'' ,direccion:'',productos:[],total:0,descuento:0},
        dniDesc:'Let us know your dni.',
        producto:{name:'',precio:0,descripcion:''},
        productos:[],
        checkedItems: [],
        boletaproductos:[],
        //descuento:0,
        fields: [ 'name', 'precio', 'opciones'],
        }
    },
    computed: {
        pagototal: function () {
            var total=0
            for (let index = 0; index < this.boleta.productos.length; index++) {
                total=total+this.boleta.productos[index].importe
            }
            return total-this.boleta.descuento
        }       
    },
    methods: {        
        form_newboleta () {
            this.boleta.total=this.pagototal;
            var url=process.env.VUE_APP_API_TEST+'boletas/store';
            axios.post(url,this.boleta).then(response=>{
                //this.resetProducto();
                toastr.success('Boleta Creado Correctamente')
                console.log(response)
            }).catch(error=> {
                console.log(error)
            });
        },
        addI(producto,vmodel){
            console.log("es: "+vmodel)
            //console.log("..")
            if (vmodel) {
                this.boleta.productos.push({id:producto.id, name:producto.name,
                precio:producto.precio, cantidad:1,
                importe:producto.precio, descripcion:''})
            console.log('add: '+producto.id)
            }else{
                this.boleta.productos = this.boleta.productos.filter(item => item.id != producto.id);
                console.log("remove id: "+producto.id)
            }
        },
        okModalProductos(){
            //Eliminar productos repetidos en boletaproductos
            var hash = {};
            this.boleta.productos = this.boleta.productos.filter(function(current) {
                var exists = !hash[current.id] || false;
                hash[current.id] = true;
                return exists;
            });
        },
        buscar_dni : function(){
            var url ='https://sitaav.org/api/certificados/dni'
            this.dniDesc="Buscando ..."
            axios.post(url,{dni:this.boleta.dni}).then(response => {
                if (response.data.user.total>=1) {
                    this.boleta.nombres=response.data.user.data[0].nombres
                    this.boleta.apepaterno=response.data.user.data[0].apepaterno
                    this.boleta.apematerno=response.data.user.data[0].apematerno
                    this.dniDesc="users encontrados :"+response.data.user.total
                }else{
                    this.dniDesc="Usuario no encontrado"
                }
            }).catch(error => {
                this.dniDesc="ocurrio un error"
                console.log(error)
            })
        },
        showModal () {
            var url=process.env.VUE_APP_API_TEST+'productos';
            axios.get(url).then(response=>{
                this.productos=response.data.productos;
            }).catch(error=> {
                console.log(error)
            });
            this.$refs.myModalRef.show()
        },
        hideModal () {
            this.$refs.myModalRef.hide()
        }
    }
}
</script>
