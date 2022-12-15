
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
                              <v-text-field type="email" solo v-model="usuario.email" :rules="emailRules" label="Informe o seu E-mail" prepend-inner-icon="mdi-email" required></v-text-field>
                            </label>

                            <label>
                              <legend>Senha:</legend>
                              <v-text-field type="password" v-model="usuario.password" :rules="senhaRules" solo label="Informe a senha" prepend-inner-icon="mdi-lock" required></v-text-field>
                            </label>

                            <label>
                              <legend>Celular:</legend>
                              <v-text-field v-model="usuario.telefone" solo 
                              
                              label="Informe o celular" 
                              :rules="[() => !!usuario.telefone || 'Esse campo é obrigatório', 
                              (usuario.telefone.length >= 9 && usuario.telefone.length <= 14) || 'Preenchar um número válido']"
                              prepend-inner-icon="mdi-lock" required></v-text-field>
                            </label>
                            
                            <label >
                              <legend>Data de Nascimento:</legend>
                              <v-text-field 
                                type="date" 
                                solo
                                v-model="usuario.dt_nascimento"
                                :rules="[() => !!usuario.dt_nascimento || 'Esse campo é obrigatório']"
                                required
                                persistent-hint
                                prepend-inner-icon="mdi-calendar"
                                @blur="date = parseDate(dateFormatted)"
                               ></v-text-field>
                            </label>
                            
                            <label>
                              <legend>CEP:</legend>
                              <v-text-field 
                              v-model="usuario.cep" solo 
                              label="Informe o CEP" 
                              prepend-inner-icon="mdi-map-marker" 
                              :rules="[() => !!usuario.cep || 'Esse campo é obrigatório']"
                              @keyup="pesquisaCEP()"
                              maxlength="8"
                              required></v-text-field>
                              
                            </label>
                            
                            
                            <label>
                              <legend>Endereço:</legend>
                              <v-text-field 
                              v-model="usuario.endereco" solo 
                              label="Informe o endereço" 
                              :rules="[() => !!usuario.endereco || 'Esse campo é obrigatório']"
                              prepend-inner-icon="mdi-map-marker" required></v-text-field>
                            </label>
                            
                            
                            <label>
                              <legend>Cidade:</legend>
                              <v-text-field v-model="usuario.cidade" solo 
                              label="Informe a cidade" 
                              :rules="[() => !!usuario.cidade || 'Esse campo é obrigatório']"
                              prepend-inner-icon="mdi-map-marker" required></v-text-field>
                            </label>
                            
                            <label>
                              <legend>Bairro:</legend>
                              <v-text-field v-model="usuario.bairro" solo 
                              :rules="[() => !!usuario.bairro || 'Esse campo é obrigatório']"
                              label="Informe a bairro" 
                              prepend-inner-icon="mdi-map-marker" required></v-text-field>
                            </label>
    
                            <p class="msg-erro">{{erro}}</p>

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
      v => (v && v.length >= 6) || 'A Senha Deve conter 6 ou mais characters',
    ],
      usuario: {
          name: null,
          email: null,
          password: null,
          perfil_id: 2,
          telefone: "",
          endereco: null,
          dt_nascimento: null,
          cep: null,
          cidade: null,
          bairro: null,
      },
      token: null,
      loading: false,
      erro: ''

  }),

  mounted() {
    this.autenticarUsuario()
    if (sessionStorage.getItem("token")) this.redirecionar();
  },
  methods: {
      redirecionar() {
        return this.$router.push({ name: "home" });
      },
      async autenticarUsuario(){
        const _this = this;
        const objAut = {email: "root@hotmail.com", password: "root"}
        const stringObjAutenticar = JSON.stringify(objAut);
        
        const req = await fetch("https://api-helpet.herokuapp.com/api/login", {
          method: "POST",
                  body: stringObjAutenticar,
                  headers: { 
                      'accept': '*/*',
                      "Content-type": "application/json"
                  }
        });
        const data = await req.json();
        if(data.token){
          _this.token = data.token
          return true
        }
        else if(data.erro){
          this.erro = data.erro;
        }
        return false
      },
      
      async salvarUsuario() {

        this.erro = ""
        for(let objUser in this.usuario){

          if(this.usuario['senha'] && this.usuario['senha'].length < 6) {
            this.erro = 'A Senha Deve conter 6 ou mais characters';
            break;
          }
          if(this.usuario[objUser] === null || this.usuario[objUser] === ""){
            this.erro = "Algum campo está vazio, preencha todos!";
            break;
          }
        }
        
        this.loading = true;
        if(this.erro === "" && await this.autenticarUsuario()){
          
          const objData = JSON.stringify(this.usuario);
          console.log(objData);
          
          const req = await fetch('https://api-helpet.herokuapp.com/api/usuario',
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
          console.log(data)
          // if(status === 201){
          //   await req.json();
          //   return this.$router.push({ name: 'login' })
          //   // console.log(data)
          // }
          // else if(status === 500){
          //   this.erro = "E-mail já cadastrado"
          // }
          this.limpaUsuario()
        }
        this.loading = false;
      },
      async pesquisaCEP(){
        let _this = this;
        this.usuario.cep =  this.usuario.cep.replace(/[^0-9.]/g, '').replace(/(\.*?)\.*/g, '$1')

        if(this.usuario.cep && this.usuario.cep.length === 8){
            const req = await fetch(`https://viacep.com.br/ws/${this.usuario.cep}/json/`);
            const data = await req.json()

            _this.usuario.endereco = data.logradouro;
            _this.usuario.cidade = data.localidade;
            _this.usuario.bairro = data.bairro;
            

        }
      },


      limpaUsuario(){
        this.usuario = {
          name: null,
          email: null,
          password: null,
          perfil_id: 2,
          telefone: "",
          endereco: null,
          dt_nascimento: null,
          cep: null,
          cidade: null,
          bairro: null,
        },
        this.erro = "";
        this.token = null;
        this.loading = false;
      }
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
    .msg-erro{
      color: red;
    }
  </style>