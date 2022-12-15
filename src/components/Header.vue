<template>
  <v-app-bar app class="cor-header header-format">
    <div class="logo d-flex align-center">
      <img src="../assets/logocatdog60.png" alt="">
      <span>
        SOS Caramelo
      </span>
    </div>
    <!-- cria um espaço -->
    <v-spacer></v-spacer>
    <div class="text-center" center>
      <v-menu offset-x>
        <template v-slot:activator="{ on, attrs }">
          <v-btn class="botao-dropdown-header" v-bind="attrs" v-on="on" @click="verificaAut">
            Menu
          </v-btn>
        </template>
        <v-list>
          <v-list-item>
            <v-list-item-title>
              <v-btn to="/" text>
                <span class="mr-2">Início</span>
              </v-btn>
            </v-list-item-title>
          </v-list-item>
          <v-list-item>
            <v-list-item-title>
              <v-btn to="/informacoes" text>
                <span class="mr-2">Informações</span>
              </v-btn>
            </v-list-item-title>
          </v-list-item>
          <v-list-item v-if="!logado">
            <v-list-item-title>
              <v-btn to="/login" text >
                <span class="mr-2">Login</span>
              </v-btn>
            </v-list-item-title>
          </v-list-item>
          <v-list-item v-if="!logado">
            <v-list-item-title>
              <v-btn to="/cadastrar" text >
                <span class="mr-2">Cadastrar-se</span>
              </v-btn>
            </v-list-item-title>
          </v-list-item>
          
          <v-list-item v-if="logado">
            <v-list-item-title>
              <v-btn @click="deslogar()">
                <span class="mr-2">Sair</span>
              </v-btn>
            </v-list-item-title>
          </v-list-item>
        </v-list>
      </v-menu>
    </div>
  </v-app-bar>
</template>

<script>
export default {
  name: "Navbar",
  data: () => ({
    logado: sessionStorage.getItem('token')? true : false
  }),

  mounted() {
  },
  methods: {
    deslogar(){
      sessionStorage.removeItem('token');
      this.logado = sessionStorage.getItem('token')? true : false
    },
    verificaAut(){
      this.logado = sessionStorage.getItem('token')? true : false
    }
  }
};


</script>


<style scoped>
  .header-format{
    display: flex;
    flex-direction: column;
    justify-content: center;
    height: 100px !important;
  }

  .logo span{
    font-family: 'Macondo Swash Cap', Times, serif !important;
  }
  .logo{
    display: flex !important;
    flex-direction: column;
  }
  .cor-header {
    background-color: #ffee1f !important;
    color: black;
  }
  .botao-dropdown-header{
    background-color: #cf9e25 !important;
    color: white !important;
  }
</style>