<template>
  <div class="app flex-row align-items-center">
    <div class="container">
      <b-row class="justify-content-center">
        <b-col md="5">
          <b-card-group>
            <b-card no-body class="p-4">
              <b-card-body>
                <b-form @submit.prevent="login_user">
                  <h1>Login</h1>
                  <p class="text-muted">Sign In to your account</p>
                  <b-input-group class="mb-3">
                    <b-input-group-prepend><b-input-group-text><i class="icon-user"></i></b-input-group-text></b-input-group-prepend>
                    <b-form-input v-model="auth.email" type="email" class="form-control" placeholder="Username" autocomplete="username email" />
                  </b-input-group>
                  <b-input-group class="mb-4">
                    <b-input-group-prepend><b-input-group-text><i class="icon-lock"></i></b-input-group-text></b-input-group-prepend>
                    <b-form-input v-model="auth.password" type="password" class="form-control" placeholder="Password" autocomplete="current-password" />
                  </b-input-group>
                  <b-row>
                    <b-col cols="6">
                      <b-button type="submit" variant="primary" class="px-4">Login</b-button>
                    </b-col>
                    <b-col cols="6" class="text-right">
                      <b-button variant="link" class="px-0">Forgot password?</b-button>
                    </b-col>
                  </b-row>
                </b-form>
              </b-card-body>
            </b-card>
            <!-- <b-card no-body class="text-white bg-primary py-5 d-md-down-none" style="width:44%">
              <b-card-body class="text-center">
                <div>
                  <h2>Sign up</h2>
                  <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>
                  <b-button @click="logout" variant="primary" class="mt-3">Register Now!</b-button>
                </div>
              </b-card-body>
            </b-card>  -->
          </b-card-group>
        </b-col>
      </b-row>
    </div>
  </div>
</template>

<script>
/*
{"success":true,"data":
{"token":
"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vc2l0YWF2LXBhZ29zLnRlc3QvYXBpL2xvZ2luIiwiaWF0IjoxNTM3MTcyNDE0LCJleHAiOjE1MzcxNzYwMTQsIm5iZiI6MTUzNzE3MjQxNCwianRpIjoiU1RZZ3VUYURPcGdKNFZCOCIsInN1YiI6MjIsInBydiI6Ijg3ZTBhZjFlZjlmZDE1ODEyZmRlYzk3MTUzYTE0ZTBiMDQ3NTQ2YWEifQ.QmBjSi9V_ad0u9GjIPPi6DtXa5jmL_dqwt5v0ovMqC0"}}
*/
export default {
  name: 'Login',

  data (){
    return{
      auth:{email:'',password:''}
    }
  },
  beforeCreate: function () {
    if (this.$session.exists()) {
      //si ya existe la sesion, validar y redirigir a home
      //aun no se como validar el token :C
     // this.$router.push({name:'Home'})
     console.log(this.$session.getAll())
    }
  },
  methods:{
    login_user(){
      var url=process.env.VUE_APP_API_TEST+'login';
      axios.post(url,this.auth).then(response=>{
          //iniciar Sesion
          this.$session.start()
          window.axios.defaults.headers.common['Authorization'] = "Bearer "+response.data.data.token
          this.$session.set('token', response.data.data.token)
          this.$session.set('user', response.data.data.user)
          this.$router.push({name:'Home'})
          /*toastr.success('Iniciando sesion')
          console.log(response.data.data.token)*/
      }).catch(error=> {
        toastr.error('Sus Credenciales Son Incorrectas')
        console.log(error)
      });
    },
    logout(){
      var url=process.env.VUE_APP_API_TEST+'logout?token='+this.$session.get('token')
      axios.get(url).then(response=>{
          //this.resetProducto();
          toastr.success('sesion Cerrada')
          this.$session.destroy()
          console.log(response)
      }).catch(error=> {
        toastr.error('error')
        console.log(error)
      });
    },
    foo(){
      var url=process.env.VUE_APP_API_TEST+'test';
      axios.get(url).then(response=>{
          //this.resetProducto();
          //toastr.success('Iniciando sesion')
          console.log(response)
      }).catch(error=> {
        toastr.error('error')
        console.log(error)
      });
    }
  }
}
</script>
