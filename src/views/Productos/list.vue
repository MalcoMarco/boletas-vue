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
                    @click="$router.push({name:'productoscreate'})">Agregar</b-button> </b-col>
                <b-col sm="12" class="table-responsive">
                    <b-table striped hover :items="productos"></b-table>
                </b-col>
            </div>
        </b-card-body>
        </b-card>
    </div>
</template>
<script>
export default {
  name: 'productocreate',
  data () {
    return {
      productos:[]
    }
  },
  created(){
      this.get_productos()
  },
  methods: {
    get_productos () {
        var url=process.env.VUE_APP_API+'productos';
        axios.get(url).then(response=>{
            this.productos=response.data.productos;
        }).catch(error=> {
            console.log(error)
        });
    }
  }
}
</script>
