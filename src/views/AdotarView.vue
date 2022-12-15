<template>
    <div class="login">
        <v-container>
            <h1>Cadastro de Animal</h1>
            <v-row class="text-center">
                <v-col offset-lg="2" lg="8" md="12">
                    <v-form>
                        <div class="input-form">
                            <v-row>
                                <v-col cols="12">
                                    <!-- <label>
                      <legend>E-mail do Usuário:</legend> -->
                                    <!-- <v-text-field solo v-model="usuario.email" label="Informar o correio eletrônico cadastrado" -->
                                    <!-- <v-text-field solo label="Informar o correio eletrônico cadastrado" prepend-inner-icon="mdi-dog"
                        required></v-text-field>
                    </label> -->
                                </v-col>
                                <v-col cols="6">
                                    <label>
                                        <legend>Nome do Animal:</legend>
                                        <v-text-field
                                            solo
                                            v-model="adocao.nome"
                                            label="Qual o nome do Bichano?"
                                            prepend-inner-icon="mdi-format-size"
                                            required
                                        ></v-text-field>
                                    </label>
                                </v-col>
                                <v-col cols="6">
                                    <label>
                                        <legend>Espécie</legend>
                                        <v-text-field
                                            required
                                            solo
                                            v-model="adocao.especie"
                                            label="Espécie"
                                            prepend-inner-icon="mdi-dog"
                                        ></v-text-field>
                                    </label>
                                </v-col>
                                <v-col cols="6">
                                    <label>
                                        <legend>Porte do Animal</legend>
                                        <v-text-field
                                            solo
                                            v-model="adocao.porte"
                                            label="Grande, Médio ou Pequeno"
                                            prepend-inner-icon="mdi-account"
                                        ></v-text-field>
                                    </label>
                                </v-col>
                                <v-col cols="6">
                                    <label>
                                        <legend>Idade</legend>
                                        <v-text-field
                                            solo
                                            v-model="adocao.idade"
                                            label="Idade"
                                            prepend-inner-icon="mdi-numeric"
                                        ></v-text-field>
                                    </label>
                                </v-col>
                            </v-row>
                            <v-row>
                                <!-- <p class="msg-erro">{{ erro }}</p> -->
                            </v-row>

                            <v-row>
                                <v-col cols="12">
                                    <v-btn
                                        class="botao-01"
                                        @click="salvarAdocao()"
                                        >Salvar</v-btn
                                    >
                                </v-col>
                            </v-row>
                        </div>
                    </v-form>
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
        adocao: {
            anunciante_usuario_id: 1,
            nome: "",
            cor: "Branco",
            especie: "",
            porte: "",
            idade: null,
        },
        token: "",
    }),
    mounted() {},
    methods: {
        async salvarAdocao() {
            this.loading = true;
            this.token = sessionStorage.getItem("token");

            const objData = JSON.stringify(this.adocao);
            const req = await fetch(
                "https://api-helpet.herokuapp.com/api/adocao",
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
            // const data
            console.log("toaqui");
            console.log(req);

            // const status = await req.json;

            this.loading = false;
            this.limparAdocao();
        },
        limparAdocao() {
            this.adocao = {
                anunciante_usuario_id: 1,
                nome: "",
                cor: "",
                especie: "",
                porte: "",
                idade: null,
            };
            this.indice = -1;
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
