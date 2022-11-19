<template>
    <v-container>
        <v-row class="text-center">
            <v-col offset-lg="2" lg="8" md="12">
                <v-form v-model="valid">
                    <v-row>
                        <v-col cols="12">

                            <v-text-field label="Usuario" v-model="usuario.login" :rules="loginRules" required></v-text-field>
                            <v-text-field v-model="usuario.email" :rules="emailRules" label="E-mail" required></v-text-field>
                            <v-text-field type="password" label="Senha" :rules="senhaRules" v-model="usuario.senha" required></v-text-field>
                        </v-col>
                    </v-row>
                    <v-row>
                        <v-col cols="12" class="text-right">
                            <v-btn class="primary" @click="salvarUsuario">Salvar</v-btn>
                        </v-col>
                    </v-row>
                </v-form>
            </v-col>
        </v-row>

        <v-row class="text-center lighten-4 blue">
            <v-col offset-lg="2" lg="8" md="12">
                <v-simple-table>
                    <thead>
                        <tr>
                            <th class="text-left" style="width: 60%">usuario</th>
                            <th class="text-left">Email</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(usuario, indice) in listaUsuario" :key="indice">
                            <td class="text-left">{{ usuario.login }}</td>
                            <td class="text-left">{{ usuario.email }}</td>
                            <td>
                                <v-btn class="orange  lighten-1 white--text" @click="alterarUsuario(usuario)">
                                    <v-icon left>mdi-wrench </v-icon>
                                    Alterar
                                </v-btn>
                                <v-btn class="red lighten-1 white--text" @click="removerUsuario(usuario.id)">
                                    <v-icon dark left>mdi-cancel </v-icon>
                                    Remover
                                </v-btn>
                            </td>
                        </tr>
                    </tbody>
                </v-simple-table>
            </v-col>
        </v-row>
    </v-container>
</template>


<script>
export default {
    name: "UsuarioForm",
    data: () => ({
        valid: true,
        loginRules: [
        v => !!v || 'Login é obrigatório',
        v => (v && v.length <= 10) || 'Login deve ser menor do que 10 caracters',
      ],
      emailRules: [
        v => !!v || 'E-mail é obrigatório',
        v => /.+@.+\..+/.test(v) || 'E-mail deve ser válido',
      ],
      senhaRules: [
        v => !!v || 'Senha é obrigatória',
        v => (v && v.length >= 8) || 'Senha Deve ser maior igual a 8 characters',
      ],
        usuario: {
            login: "",
            email: "",
            senha: "",
        },
        listaUsuario: [],
        indice: -1,
    }),
    mounted() {
        this.getUsuario();
    },
    methods: {
        async getUsuario() {
            const req = await fetch('http://localhost:3000/usuario');
            const data = await req.json();
            this.listaUsuario = data;
        },
        limpaUsuario() {
            this.usuario = {
                id: 0,
                usuario: "",
                email: "",
                senha: ""
            };
            this.indice = -1;
        },
        async salvarUsuario() {
            
            if(this.indice < 0){
                
                const objData = JSON.stringify(this.usuario);
                const req = await fetch('http://localhost:3000/usuario',
                {
                    method: "POST",
                    body: objData,
                    headers: { 
                        "Content-type": "application/json"
                    }
                });
                const data = await req.json();
                console.log(data);

            }else{

                const objData = JSON.stringify(this.usuario);
                this.alterarUsuario(this.usuario);
                const req = await fetch('http://localhost:3000/usuario/'+ this.indice,
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

            this.getUsuario();
            this.limpaUsuario();
        },

        async alterarUsuario(user){
            this.indice = user.id;
            this.usuario = user;

        },
        async removerUsuario(indice){
        const req = await fetch('http://localhost:3000/usuario/'+indice,
        {
            method: "DELETE",
            headers: { 
                "Content-type": "application/json"
            }
        });
        const data = await req.json();
        console.log(data);
        this.getUsuario();

        }
    }

};
</script>

<style scoped>

</style>