<template>
    <div class="login">
        <v-container>
            <h1>Login</h1>
            <v-row class="text-center">
                <v-col offset-lg="2" lg="8" md="12">
                    <v-form v-model="valid">
                        <div class="input-form">
                            <v-row>
                                <v-col cols="12">
                                    <label>
                                        <legend>Usuário:</legend>
                                        <v-text-field
                                            solo
                                            v-model="usuario.email"
                                            label="Informe Usuário ou E-mail"
                                            prepend-inner-icon="mdi-account"
                                            required
                                        ></v-text-field>
                                    </label>

                                    <label>
                                        <legend>Senha:</legend>
                                        <v-text-field
                                            type="password"
                                            v-model="usuario.password"
                                            solo
                                            label="Informe a senha"
                                            prepend-inner-icon="mdi-lock"
                                            required
                                        ></v-text-field>
                                    </label>
                                </v-col>
                            </v-row>
                            <v-row>
                                <p class="msg-erro">{{ erro }}</p>
                            </v-row>

                            <v-row>
                                <v-col cols="12">
                                    <v-btn
                                        class="botao-01"
                                        @click="autenticarUsuario"
                                        >ACESSAR</v-btn
                                    >

                                    <div class="bloco-texto-inscreva">
                                        <p class="font-legenda">
                                            Não tem conta?
                                            <v-btn to="/cadastrar" text>
                                                Inscreva-se</v-btn
                                            >
                                        </p>
                                    </div>
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
    name: "UsuarioForm",
    data: () => ({
        usuario: {},
        erro: null,
        valid: true,
        listaUsuario: [],
        indice: -1,
    }),
    mounted() {
        if (sessionStorage.getItem("token")) this.redirecionar();
    },
    methods: {
        redirecionar() {
            return this.$router.push({ name: "home" });
        },
        async autenticarUsuario() {
            const objAut = this.usuario;
            console.log(objAut);
            const stringObjAutenticar = JSON.stringify(objAut);

            const req = await fetch(
                "http://api-helpet.herokuapp.com/api/login",
                {
                    method: "POST",
                    body: stringObjAutenticar,
                    headers: {
                        accept: "*/*",
                        "Content-type": "application/json",
                    },
                }
            );
            const data = await req.json();
            if (data.token) {
                sessionStorage.setItem("token", data.token);
                this.erro = null;
                return this.$router.push({ name: "home" });
            } else if (data.erro) {
                console.log(data.erro);
                this.erro = data.erro;
            }
        },

        // async getUsuario() {
        //     const req = await fetch('http://localhost:3000/usuario');
        //     const data = await req.json();
        //     this.listaUsuario = data;
        // },
        // limpaUsuario() {
        //     this.usuario = {
        //         id: 0,
        //         usuario: "",
        //         email: "",
        //         senha: ""
        //     };
        //     this.indice = -1;
        // },
        // async salvarUsuario() {

        //     if(this.indice < 0){

        //         const objData = JSON.stringify(this.usuario);
        //         const req = await fetch('http://localhost:3000/usuario',
        //         {
        //             method: "POST",
        //             body: objData,
        //             headers: {
        //                 "Content-type": "application/json"
        //             }
        //         });
        //         const data = await req.json();
        //         console.log(data);

        //     }else{

        //         const objData = JSON.stringify(this.usuario);
        //         this.alterarUsuario(this.usuario);
        //         const req = await fetch('http://localhost:3000/usuario/'+ this.indice,
        //         {
        //             method: "PUT",
        //             body: objData,
        //             headers: {
        //                 "Content-type": "application/json"
        //             }
        //         });
        //         const data = await req.json();
        //         this.listaUsuario = data;
        //     }

        //     this.getUsuario();
        //     this.limpaUsuario();
        // },

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
