<template>
  <AppHeaderDropdown right no-caret>
    <template slot="header">
      <img
        :src="this.$session.get('user').avatar"
        class="img-avatar"
        alt="admin" />
    </template>\
    <template slot="dropdown">
      <b-dropdown-header tag="div" class="text-center"><strong>Account</strong></b-dropdown-header>
      <b-dropdown-item><i class="fa fa-envelope-o" /> Messages
      </b-dropdown-item>
      <!-- <b-dropdown-item><i class="fa fa-tasks" /> Tasks
        <b-badge variant="danger">{{ itemsCount }}</b-badge>
      </b-dropdown-item>
      <b-dropdown-item><i class="fa fa-comments" /> Comments
        <b-badge variant="warning">{{ itemsCount }}</b-badge>
      </b-dropdown-item> -->
      <!-- <b-dropdown-header
        tag="div"
        class="text-center">
        <strong>Settings</strong>
      </b-dropdown-header> -->
      <b-dropdown-item><i class="fa fa-user" /> Perfil</b-dropdown-item>
      <!-- <b-dropdown-item><i class="fa fa-wrench" /> Settings</b-dropdown-item>
      <b-dropdown-item><i class="fa fa-usd" /> Payments
        <b-badge variant="secondary">{{ itemsCount }}</b-badge>
      </b-dropdown-item>
      <b-dropdown-item><i class="fa fa-file" /> Projects
        <b-badge variant="primary">{{ itemsCount }}</b-badge>
      </b-dropdown-item> -->
      <!-- <b-dropdown-divider /> -->
      <b-dropdown-item @click.prevent="logout"><i class="fa fa-lock" /> Logout</b-dropdown-item>
    </template>
  </AppHeaderDropdown>
</template>

<script>
import { HeaderDropdown as AppHeaderDropdown } from '@coreui/vue'
export default {
  name: 'DefaultHeaderDropdownAccnt',
  components: {
    AppHeaderDropdown
  },
  data: () => {
    return { itemsCount: 42 }
  },
  methods:{
    user(){
      console.log(this.$session.get('user').avatar)
    },
    logout(){
      var url=process.env.VUE_APP_API_TEST+'logout?token='+this.$session.get('token')
      axios.get(url).then(response=>{
          //this.resetProducto();
          toastr.success('sesion Cerrada')
          this.$session.destroy()
          this.$router.push({name:'Login'})          
      }).catch(error=> {
        toastr.error('error Logout')
        console.log('Logout: '+error)
      });
    },
  }
}
</script>
