<template>
  <v-container>
    <div class="text-center" center>
      <v-dialog v-model="dialog" width="650">
        <template v-slot:activator="{ on, attrs }">
          <v-btn-toggle mandatory center>
            <v-btn
              @click="limpaAnime"
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
              Novo Anime
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
            Informações do Anime
          </v-card-title>

          <v-card-text class="green lighten-5">
            <v-spacer>.</v-spacer>
            <v-row class="text-center green lighten-5" rounded>
              <v-col offset-lg="2" lg="8" md="12">
                <v-form v-model="valid">
                  <v-row>
                    <v-col cols="12">
                      <v-text-field label="Nome" v-model="anime.nome"></v-text-field>
                      <v-text-field
                        label="Diretor"
                        v-model="anime.diretor"
                      ></v-text-field>
                      <v-text-field
                        label="Estudio"
                        v-model="anime.estudio"
                      ></v-text-field>
                      <v-text-field
                        v-model="anime.qtdtemporadas"
                        type="number"
                        label="Quantidade de temporadas"
                      >
                      </v-text-field>

                      <v-text-field
                        v-model="anime.imgcapa"
                        type="url"
                        label="Url da Imagem"
                      >
                      </v-text-field>
                      
                      <v-textarea
                        v-model="anime.sinopse"
                        label="Sinopse"
                        value="Digite aqui a sinopse"
                      >
                      </v-textarea>

                      <v-select
                        v-model="addCategoria"
                        :items="categorias"
                        label="Categorias"
                      >
                      </v-select>
                      <v-btn text @click="setCategoria">
                        <v-icon>mdi-plus</v-icon>
                        <span class="mr-2">Categoria</span>
                      </v-btn>
                      <table style="margin:0 auto">
                        <tr 
                          v-for="(cat, indice) in anime.categorias"
                          :key="indice"
                          class="text-left"
                        >
                          <td>{{ cat }}</td>
                          <td style="padding-left: 10px">
                            <v-btn-toggle rounded>
                              <v-btn
                                class="red darken-1 white--text small"
                                @click="removeCategoria(indice)"
                              >
                                <v-icon dark center>mdi-cancel</v-icon></v-btn
                              >
                            </v-btn-toggle>
                          </td>
                          
                        </tr>

                        <p></p>
                      </table>

                      <v-select
                        v-model="addPlataforma"
                        :items="plataformas"
                        label="Plataformas"
                      >
                      </v-select>
                      <v-btn text @click="setPlataforma">
                        <v-icon>mdi-plus</v-icon>
                        <span class="mr-2">Plataforma</span>
                      </v-btn>

                      <table style="margin:0 auto">
                        <tr v-for="(plat, indice) in anime.plataformas" :key="indice">
                          <td>
                            {{ plat }}
                          </td>
                          <td  style="padding-left: 10px">
                            <v-btn-toggle rounded>
                              <v-btn
                                class="red darken-1 white--text small"
                                @click="removePlataforma(indice)"
                              >
                                <v-icon dark center>mdi-cancel </v-icon></v-btn
                              >
                            </v-btn-toggle>  
                          </td>
                        </tr>
                      </table>

                      <v-text-field
                        type="date"
                        label="Data de lançamento"
                        v-model="anime.dtlancamento"
                      ></v-text-field>
                      <v-text-field
                        type="number"
                        label="Quantidade de episódios"
                        v-model="anime.qtdEp"
                      ></v-text-field>
                      <v-select
                        v-model="anime.statusDoAnime"
                        :items="statusDoAnime"
                        label="Status Do Anime"
                      >
                      </v-select>
                      <v-text-field
                        type="number"
                        label="Nota"
                        v-model="anime.nota"
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
              @click="salvarAnime"
            >
              Salvar
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </div>
    <!-- <v-space>.</v-space> -->
    <br>
    <br>
    <v-row v-show="!hidden">
      <v-col offset-lg="2" lg="8" md="12">
          <div>
            <v-text-field
              label="Pesquisa..."
              v-model="search"
              @keypress.enter="searchAnime"
              @keyup="searchAnime"
            ></v-text-field>
          </div>
      </v-col>
    </v-row>

    <!-- bloco do código para a exibição dos animes -->
    <v-row class="text-center" v-show="!hidden">
      <v-col offset-lg="1" lg="10" ms="12">
              <div v-if="listaAnime.length == 0"> <h1>Não foi encontrado nenhum registro!</h1></div>

              <div class="bloco--anime" v-for="(anime, indice) in listaAnime" :key="indice">
                
                
                <v-card class="mx-auto">
                  <template slot="progress">
                    <v-progress-linear
                      color="deep-purple"
                      height="10"
                      indeterminate
                    ></v-progress-linear>
                  </template>

                  <!-- bloco do código referente a imagem -->
                  <v-img
                    height="150"
                    :src="anime.imgcapa"
                    >
                    <!-- src="https://i.pinimg.com/736x/58/53/83/585383da22c40c31cea4d9181df39ff1.jpg" -->
                    <v-card-actions>
                      <v-btn-toggle center align="center" justify="center">
                        <v-btn
                          class="yellow darken white--text center"
                          @click="alterarAnime(anime)"
                        >
                          <v-icon center>mdi-wrench</v-icon>
                        </v-btn>

                        <v-btn
                          class="yellow darken-2 center"
                          @click="removerAnime(anime.id)"
                          center
                        >
                          <v-icon center>mdi-cancel</v-icon>
                        </v-btn>
                      </v-btn-toggle>
                    </v-card-actions>
                  </v-img>

                  <v-card-title>{{ anime.nome }}</v-card-title>

                  <v-card-text>
                    <v-row align="center" class="mx-0"> </v-row>

                    <div class="my-4 text-subtitle-1">
                      {{ anime.estudio }} {{ anime.statusDoAnime }}
                      {{ anime.dtlancamento }} |
                    </div>
                    <div>
                      {{ caracteresReduzido(anime.sinopse, 70) }}
                    </div>
                  </v-card-text>
                  <v-divider class="mx-4"></v-divider>
                  <v-card-text>
                    <v-row align="center" class="mx-0"> </v-row>

                    <div class="my-4 text-subtitle-1">
                      {{ anime.diretor }}
                    </div>
                    <div>
                      Disponível: <strong>{{ converteListaString(anime.plataformas) }}</strong> <br />
                      Com {{ anime.qtdtemporadas }} temporadas e {{ anime.qtdEp }} de
                      episódios.<br />Categorias: <strong>{{ converteListaString(anime.categorias) }}</strong> <br />Avaliação:
                    </div>
                  </v-card-text>

                </v-card>
                
              </div>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "AdotarForm",
  data: () => ({
    hidden: true,
    dialog: false,
    valid: true,
    anime: {
      nome: "",
      diretor: "",
      estudio: "",
      qtdtemporadas: "",
      sinopse: "",
      categorias: [],
      plataformas: [],
      dtlancamento: "",
      qtdEp: "",
      statusDoAnime: "",
      nota: "",
      imgcapa: "",
    },
    errors: "",
    addCategoria: "",
    categorias: [],
    addPlataforma: "",
    plataformas: [],
    itemsQtdTemp: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20],
    statusDoAnime: ["Finalizado", "Em lançamento", "Em produção"],
    listaAnime: [],
    indice: -1,
    search: "",
  }),
  mounted() {
    this.getAnime();
    this.getCategorias();
    this.getPlataformas();
  },
  methods: {
    async searchAnime() {
      const req = await fetch("http://localhost:3000/anime?q=" + this.search);
      const data = await req.json();
      this.listaAnime = data;
    },
    setCategoria() {
      if (!this.anime.categorias.includes(this.addCategoria) && this.addCategoria != "") {
        this.anime.categorias.push(this.addCategoria);
        this.addCategoria = "";
      } else {
        this.errors = "Não deve repetir a mesma categoria";
      }
    },
    // função que limitar os caracteres
    caracteresReduzido(texto, qtd){
      
       let lista = texto.slice(0,qtd);
       lista += "...";
       return lista;
    },
    converteListaString(lista){
      let string = "";
      if(lista?.length > 0){
        for(let i =0 ; lista?.length > i ; i++){
          string += lista[i];
 
          if(lista?.length - 1 == i ) string += "."
          else string += ", ";
        }
      }
      
      return string;

    },
    removeCategoria(indice) {
      this.anime.categorias.splice(indice, 1);
    },
    setPlataforma() {
      if (
        !this.anime.plataformas.includes(this.addPlataforma) &&
        this.addPlataforma != ""
      ) {
        this.anime.plataformas.push(this.addPlataforma);
        this.addPlataforma = "";
      } else {
        this.errors = "Não deve repetir a mesma plataforma";
      }
    },
    removePlataforma(indice) {
      this.anime.plataformas.splice(indice, 1);
    },
    async getCategorias() {
      const req = await fetch("http://localhost:3000/categorias");
      const data = await req.json();
      data.map((data) => {
        this.categorias.push(data.nome);
      });
    },
    async getPlataformas() {
      const req = await fetch("http://localhost:3000/plataforma");
      const data = await req.json();
      data.map((data) => {
        this.plataformas.push(data.plataforma);
      });
    },
    async getAnime() {
      const req = await fetch("http://localhost:3000/anime");
      const data = await req.json();
      this.listaAnime = data;
    },
    limpaAnime() {
      this.anime = {
        id: 0,
        nome: "",
        diretor: "",
        estudio: "",
        qtdtemporadas: "",
        sinopse: "",
        categorias: [],
        plataformas: [],
        dtlancamento: "",
        qtdEp: "",
        statusDoAnime: "",
        nota: "",
        imgcapa: ""
      };
      this.indice = -1;
    },
    async salvarAnime() {
      if (this.indice < 0) {
        const objData = JSON.stringify(this.anime);
        const req = await fetch("http://localhost:3000/anime", {
          method: "POST",
          body: objData,
          headers: {
            "Content-type": "application/json",
          },
        });
        const data = await req.json();
        console.log(data);
      } else {
        const objData = JSON.stringify(this.anime);
        this.alterarAnime(this.anime);
        const req = await fetch("http://localhost:3000/anime/" + this.indice, {
          method: "PUT",
          body: objData,
          headers: {
            "Content-type": "application/json",
          },
        });
        const data = await req.json();
        this.listaAnime = data;
      }

      this.getAnime();
      this.limpaAnime();
      this.dialog = false;
    },

    async alterarAnime(anime) {
      this.indice = anime.id;
      this.anime = anime;
      this.dialog = true;
    },
    async removerAnime(indice) {
      const req = await fetch("http://localhost:3000/anime/" + indice, {
        method: "DELETE",
        headers: {
          "Content-type": "application/json",
        },
      });
      const data = await req.json();
      console.log(data);
      this.getAnime();
    },
  },
};
</script>

<style>
/* BARRA DE ROLAGEM */
div::-webkit-scrollbar {
  width: 8px; /* width of the entire scrollbar */
}

div::-webkit-scrollbar-track {
  background: rgb(255, 255, 255); /* color of the tracking area */
}

div::-webkit-scrollbar-thumb {
  background-color: #cf9e25; /* color of the scroll thumb */
  border-radius: 0 20px 0 20px; /* roundness of the scroll thumb */
  /*border: 3px solid #ffeb3b;  /*creates padding around scroll thumb*/
  border: 1px solid #fde402; /*creates padding around scroll thumb*/
}
div::-webkit-scrollbar-thumb:hover {
  background-color: #fde402; /* color of the scroll thumb */
  border-radius: 0 20px 0 20px; /* roundness of the scroll thumb */
  /* border: 3px solid orange;  creates padding around scroll thumb */
}
.bloco--anime{
  display: block;
  max-height: 500px;
  min-height: 500px;
  width: 33.3334%;
  float: left;
  margin-bottom: 10px;
}
.bloco--anime .v-card__actions {
  /* display: flex !important; */
  justify-content: flex-end;
}
.bloco--anime .v-card{
  height: 500px;
  max-width: 90%;
  
}

@media  (max-width: 768px) {
  .bloco--anime{
    width: 100%;
    margin: 0;
    margin-bottom: 10px;
  }
}
</style>
