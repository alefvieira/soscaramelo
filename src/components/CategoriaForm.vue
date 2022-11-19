<template>
  <v-container>
    <div class="text-center" center>
      <v-dialog v-model="dialog" width="500">
        <template v-slot:activator="{ on, attrs }">
          <v-btn-toggle mandatory center>
            <v-btn
            @click="limpaCategoria"
            center
              rounded
              elevation="2"
              color="yellow darken-2"
              dark
              large
              class="text-center black--text"
              v-bind="attrs"
              v-on="on"
              
            >
              Nova Categoria
            </v-btn>
            <v-spacer></v-spacer>
            <v-spacer></v-spacer>
            <v-btn
              center
              rounded
              elevation="2"
              color="yellow darken-2"
              dark
              large
              class="text-center black--text"
              @click="hidden = !hidden"
            >
              {{ hidden ? "Cadastrados" : "Ocultar" }}
            </v-btn>
          </v-btn-toggle>
        </template>

        <v-card>
          <v-card-title
            class="justify-center text-h5 black--text yellow darken-2 rounded"
            rounded
          >
            Cadastro
          </v-card-title>
          <v-card-text class="green lighten-5">
            <v-spacer>.</v-spacer>
            <v-row class="text-center green lighten-5" rounded>
              <v-col offset-lg="2" lg="8" md="12">
                <v-form v-model="valid">
                  <v-row>
                    <v-col cols="12">
                      <v-text-field
                        label="Categoria"
                        v-model="categoria.nome"
                        @keypress.enter="salvarCategoria"
                      ></v-text-field>
                    </v-col>
                  </v-row>
                </v-form>
              </v-col>
            </v-row>
          </v-card-text>
          <v-divider></v-divider>

          <v-card-actions color="blue darken-4">
            <v-spacer></v-spacer>
            <v-btn
              block
              rounded
              elevation="2"
              color="blue darken-4"
              class="white--text"
              @click="salvarCategoria"
            >
              Salvar
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </div>

    <v-row v-show="!hidden">
      <v-col offset-lg="2" lg="8" md="12">
          <div>
            <v-text-field
              label="Pesquisa..."
              v-model="search"
              @keypress.enter="searchCategoria"
              @keyup="searchCategoria"
            ></v-text-field>
          </div>
      </v-col>
    </v-row>

    <v-row v-show="!hidden">
      <v-col offset-lg="2" lg="8" md="12">
        <div v-if="listaCategoria.length == 0"> <h1>Não foi encontrado nenhum registro!</h1></div>
        <div v-else>
          <v-simple-table>
            <thead>
              <tr>
                <th class="text-left" style="width: 80%">Categoria</th>
                <th class="text-left" style="width: 20%">Ação</th>
              </tr>
            </thead>
            <tbody>
              <tr
                v-for="(categoria, indice) in listaCategoria"
                :key="indice"
                :class="categoria.feita ? 'lighten-4 orange' : ''"
              >
                <td class="text-left">
                  <div class="sty--categ">
                    {{ categoria.nome }}
                  </div>
                </td>
                <td align="right">
                  <v-btn-toggle center align="right" justify="right">
                    <v-btn
                      class="yellow darken white--text right"
                      @click="alterarCategoria(categoria)"
                    >
                      <v-icon center>mdi-wrench</v-icon>
                    </v-btn>
  
                    <v-btn
                      class="yellow darken-2 right"
                      @click="removerCategoria(categoria.id)"
                      center
                    >
                      <v-icon center>mdi-cancel</v-icon>
                    </v-btn>
                  </v-btn-toggle>
                </td>
              </tr>
            </tbody>
          </v-simple-table>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "CategoriaForm",
  data: () => ({
    categoria: {
      id: 0,
      nome: "",
    },
    listaCategoria: [],
    indice: -1,
    search: "",
    hidden: true,
    dialog: false,
    valid: true,
  }),
  mounted() {
    this.getCategorias();
  },
  methods: {
    async getCategorias() {
      const req = await fetch("http://localhost:3000/categorias");
      const data = await req.json();
      this.listaCategoria = data;
    },
    async searchCategoria() {
      const req = await fetch("http://localhost:3000/categorias?q=" + this.search);
      const data = await req.json();
      this.listaCategoria = data;
    },
    limpaCategoria() {
      this.categoria = {
        id: 0,
        nome: "",
      };
      this.indice = -1;
    },
    async salvarCategoria() {
      if (this.indice < 0) {
        const objData = JSON.stringify(this.categoria);
        const req = await fetch("http://localhost:3000/categorias", {
          method: "POST",
          body: objData,
          headers: {
            "Content-type": "application/json",
          },
        });
        const data = await req.json();
        console.log(data);
      } else {
        const objData = JSON.stringify(this.categoria);
        this.alterarCategoria(this.categoria);
        const req = await fetch("http://localhost:3000/categorias/" + this.indice, {
          method: "PUT",
          body: objData,
          headers: {
            "Content-type": "application/json",
          },
        });
        const data = await req.json();
        this.listaCategoria = data;
      }

      this.getCategorias();
      this.limpaCategoria();
      this.dialog = false;
      this.limpaCategoria();
    },

    async alterarCategoria(categ) {
      this.indice = categ.id;
      this.categoria = categ;
      this.dialog = true;
    },
    async removerCategoria(indice) {
      const req = await fetch("http://localhost:3000/categorias/" + indice, {
        method: "DELETE",
        headers: {
          "Content-type": "application/json",
        },
      });
      const data = await req.json();
      console.log(data);
      this.getCategorias();
    },
  },
};
</script>

<style>
  .sty--categ{
    background-color: #388e3c;
    padding: 5px;
    border-radius:5px ;
    font-size: 16px;
    color: white;
    text-align: center;


  }
</style>
