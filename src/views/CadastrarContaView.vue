
<template>
    <div class="cadastro-conta">
      <v-container>
        <h1>Cadastre aqui a sua conta</h1>
        <v-row class="text-center">
            <v-col offset-lg="2" lg="8" md="12">
                <v-form v-model="valid">
                  <div class="input-form">
                    <v-row>
                        <v-col cols="12">
                            <label>
                              <legend>Usuário:</legend>
                              <v-text-field solo v-model="usuario.name" :rules="usuarioRules" label="Informe Usuário" prepend-inner-icon="mdi-account" required></v-text-field>
                            </label> 

                            <label>
                              <legend>E-mail:</legend>
                              <v-text-field solo v-model="usuario.email" :rules="emailRules" label="Informe o seu E-mail" prepend-inner-icon="mdi-email" required></v-text-field>
                            </label>

                            <label>
                              <legend>Senha:</legend>
                              <v-text-field type="password" v-model="usuario.password" :rules="senhaRules" solo label="Informe a senha" prepend-inner-icon="mdi-lock" required></v-text-field>
                            </label>
    

                            <!-- <v-text-field label="Usuario" v-model="usuario.login" :rules="loginRules" required></v-text-field>
                            <v-text-field v-model="usuario.email" :rules="emailRules" label="E-mail" required></v-text-field>
                            <v-text-field type="password" label="Senha" :rules="senhaRules" v-model="usuario.senha" required></v-text-field> -->
                        </v-col>
                    </v-row>
                    <v-row>
                        <v-col cols="12">
                            <v-btn v-if="!loading" class="botao-01" @click="salvarUsuario">CADASTRAR</v-btn>
                            <v-btn v-else class="botao-01" @click="salvarUsuario" disabled>AGUARDE...</v-btn>
                        </v-col>
                    </v-row>
                  </div>
                </v-form>
            </v-col>
        </v-row>

      </v-container>
    </div>
  </template>
  

  
<script>
export default {
  name: "UsuarioForm",
  data: () => ({
      valid: true,
      
    usuarioRules: [
      v => !!v || 'O Usuário é obrigatório',
      v => (v && v.length >= 2) || 'O Usuário deve conter mais de uma letra',
    ],
    emailRules: [
      v => !!v || 'E-mail é obrigatório',
      v => /.+@.+\..+/.test(v) || 'E-mail deve ser válido',
    ],
    senhaRules: [
      v => !!v || 'Senha é obrigatória',
      v => (v && v.length >= 6) || 'A Senha Deve conter 8 ou mais characters',
    ],
      usuario: {
          name: "",
          email: "",
          password: "",
          perfil_id: 2
      },
      token: null,
      indice: -1,
      loading: false
  }),

  mounted() {
  },
  methods: {
      async autenticarUsuario(){
        const objAut = {email: "root@hotmail.com", password: "root"}
        const stringObjAutenticar = JSON.stringify(objAut);
        
        const req = await fetch("http://api-helpet.herokuapp.com/api/login", {
          method: "POST",
                  body: stringObjAutenticar,
                  headers: { 
                      'accept': '*/*',
                      "Content-type": "application/json"
                  }
        });
        const data = await req.json();
        if(data.token){
          this.token = data.token;
        }
        else if(data.erro){
          console.log(data.erro);
          this.erro = data.erro;
        }
       
      },
      
      async salvarUsuario() {
          
          if(this.indice < 0){
            this.autenticarUsuario()
              if(this.token){
                this.loading = true;
                const objData = JSON.stringify(this.usuario);
                const req = await fetch('http://api-helpet.herokuapp.com/api/usuario',
                {
                    method: "POST",
                    body: objData,
                    headers: { 
                        "Content-type": "application/json",
                        'accept': '*/*',
                        "Authorization": `Bearer ${this.token}`
                    }
                });
                const data = await req.json();
                if(data.erro){
                  console.log(data.erro)
                }
                console.log(data.erro)

              }

          }else{

              const objData = JSON.stringify(this.usuario);
              this.alterarUsuario(this.usuario);
              const req = await fetch('http://api-helpet.herokuapp.com/api/usuario'+ this.indice,
              {   
                  method: "PUT",
                  body: objData,
                  headers: {
                      "Content-type": "application/json"
                  }
              });
              const data = await req.json();
              this.listaUsuario = data;
          }
          this.loading = false;

          this.limpaUsuario();
      },
      limpaUsuario(){
        this.usuario = {
          name: "",
          email: "",
          password: "",
          perfil_id: 2
        },
      this.token = null,
      this.indice = -1,
      this.loading = false
      }
      // async alterarUsuario(user){
      //     this.indice = user.id;
      //     this.usuario = user;

      // },
      // async removerUsuario(indice){
      // const req = await fetch('http://localhost:3000/usuario/'+indice,
      // {
      //     method: "DELETE",
      //     headers: { 
      //         "Content-type": "application/json"
      //     }
      // });
      // const data = await req.json();
      // console.log(data);
      // this.getUsuario();

      // }
  }

};
</script>


  <style>
    .cadastro-conta h1{
      text-align: center;
    }
    .cadastro-conta{
      display: flex;
      padding: 110px  2% 0 2%;
      widows: 100%;
      height: 100%;
      background-color: #ffee1f;
      align-content: center;
      align-items: center;
    }
    
    .input-form{
      padding-top: 20px;
    }
    .input-form .v-input__slot input{
      background-color: #f1e56b;
    }
    .input-form .v-input__slot{
      background-color: #f1e56b !important;
      border-radius: 20px;
    }
    .input-form .v-input__slot .v-icon{
      color: #474545 !important;
    }
    .input-form label legend{
      text-align: left;
      font-size: 14px;
      font-weight:  bold;
      padding: 10px 0 5px 15px;
    }
    .input-form .botao-01{
      margin: 0 auto;
      min-width: 130px;
      width: 50%;
      background-color: #404140 !important;
      border-radius: 20px;
      color: white;
    }
  </style>