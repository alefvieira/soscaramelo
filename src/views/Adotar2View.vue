<template>
    <div class="login">
        <v-container>
            <h1>ANIMAIS DISPONÍVEIS</h1>
            <v-row class="text-center">
                <v-col offset-lg="1" lg="10" ms="12">
                    <!-- <div v-if="listaAnime.length == 0"> <h1>Não foi encontrado nenhum registro!</h1></div> -->

                    <!-- <div class="bloco--anime" v-for="(anime, indice) in listaAnime" :key="indice"> -->

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
                            width="500"
                            src="https://memetizando.com.br/wp-content/uploads/2018/04/GIF-CACHORRO-BUGADO.gif"
                        >
                            <v-card-actions>
                                <v-btn-toggle
                                    center
                                    aligner="center"
                                    justify="center"
                                >
                                    <v-btn
                                        class="yellow darken white--text center"
                                    >
                                        <v-icon center>mdi-wrench</v-icon>
                                    </v-btn>

                                    <v-btn
                                        class="yellow darken-2 center"
                                        center
                                    >
                                        <v-icon center>mdi-cancel</v-icon>
                                    </v-btn>
                                </v-btn-toggle>
                            </v-card-actions>
                        </v-img>

                        <!-- <v-card-title>PLADEH{{ anime.nome }}</v-card-title> -->
                        <v-card-title>|| NOME DO ANIMAL ||</v-card-title>
                        <v-card-text>
                            <v-row aligner="center" class="mx-0"> </v-row>

                            <div class="my-4 text-subtitle-1">
                                <!-- {{ anime.estudio }} {{ anime.statusDoAnime }}
                        {{ anime.dtlancamento }} | -->
                            </div>
                            <div>
                                Oi, eu me chamo {adocao.nome}, tenho
                                {adocao.idade} sendo do porte {adocao.porte},
                                será que você tens o que é preciso para me dar
                                amor e carinho.
                                <!-- {{ caracteresReduzido(anime.sinopse, 70) }} -->
                            </div>
                        </v-card-text>
                        <v-divider class="mx-4"></v-divider>
                        <v-card-text>
                            <v-row aligner="center" class="mx-0"> </v-row>

                            <div class="my-4 text-subtitle-1">
                                Entre em contato com o
                                <!-- {{ anime.diretor }} -->
                            </div>
                            PLACEHOLDER
                        </v-card-text>
                    </v-card>

                    <!-- </div> -->
                </v-col>
            </v-row>
        </v-container>
    </div>
</template>

<script></script>

<script>
export default {
    name: "AdotarForm",
    data: () => ({
        loading: false,
    }),
    mounted() {
        // if (sessionStorage.getItem("token")) this.redirecionar();
    },
    methods: {
        async salvarAdocao() {
            this.loading = true;
            if (await this.autenticarUsuario()) {
                const objData = JSON.stringify(this.usuario);
                const req = await fetch(
                    "https://api-helpet.herokuapp.com/api/usuario",
                    {
                        method: "POST",
                        body: objData,
                        headers: {
                            "Content-type": "application/json",
                            accept: "*/*",
                            Authorization: `Bearer ${this.token}`,
                        },
                    }
                );

                // const status = await req.json;
            }

            this.loading = false;
        },
    },
};
</script>

<style>
.login h1 {
    text-align: center;
}

.login {
    display: flex;
    padding: 110px 2% 0 2%;
    widows: 100%;
    height: 100%;
    background-color: #ffee1f;
    align-content: center;
    align-items: center;
}

.input-form {
    padding-top: 20px;
}

.input-form .v-input__slot input {
    background-color: #f1e56b;
}

.input-form .v-input__slot {
    background-color: #f1e56b !important;
    border-radius: 20px;
}

.input-form .v-input__slot .v-icon {
    color: #474545 !important;
}

.input-form label legend {
    text-align: left;
    font-size: 14px;
    font-weight: bold;
    padding: 10px 0 5px 15px;
}

.input-form .botao-01 {
    margin: 0 auto;
    min-width: 130px;
    width: 50%;
    background-color: #404140 !important;
    border-radius: 20px;
    color: white;
}

.bloco-texto-inscreva {
    width: 50%;
    margin: 0 auto;
}

.font-legenda a {
    font-weight: bold;
    text-transform: lowercase !important;
}

.font-legenda {
    text-align: right;
    font-size: 15px;
    color: #404140 !important;
}

.msg-erro {
    color: red;
}

@media screen and (max-width: 768px) {
    .bloco-texto-inscreva {
        width: 100%;
        margin: 0 auto;
    }

    .font-legenda {
        text-align: center;
    }

    .input-form .botao-01 {
        width: 70%;
    }
}
</style>
