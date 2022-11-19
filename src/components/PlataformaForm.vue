<template>
    <v-container>
        <v-row class="text-center">
            <v-col offset-lg="2" lg="8" md="12">
            <v-row>
            <v-col cols="12">
            <v-text-field label="Plataforma" v-model="plataforma.plataforma" ></v-text-field>
            <v-select
                v-model="plataforma.tipoServico"
                :items="items"
                label="Tipo de Serviço">
            </v-select>


            </v-col>
            </v-row>
            <v-row>
            <v-col cols="12" class="text-right">
                <v-btn class="primary" @click="salvarPlataforma">Salvar</v-btn>
            </v-col>
            </v-row>
            </v-col>
            </v-row>
            <v-row class="text-center lighten-4 blue">
                <v-col offset-lg="2" lg="8" md="12">
                    <div>
                        <v-text-field 
                            label="Pesquisa..." 
                            v-model="search" 
                            @keypress.enter="searchPlataforma"
                            @keyup="searchPlataforma">
                        </v-text-field>
                    </div>

                </v-col>
            <v-col offset-lg="2" lg="8" md="12">
                <div v-if="listaPlataforma.length == 0"> <h1>Não foi encontrado nenhum registro!</h1></div>
                <div v-else>
                    <v-simple-table>
                        <thead>
                        <tr>
                            <th class="text-left" style="width: 60%">Plataforma</th>
                            <th class="text-left">Tipo Serviço</th>
                            <th class="text-left">Ação</th>
                        </tr>
                        </thead>
                        <tbody>
                            <tr v-for="(plataforma, indice) in listaPlataforma"
                                :key="indice"
                                :class="plataforma.feita ? 'lighten-4 orange' : ''"
                            >
                                <td class="text-left">{{ plataforma.plataforma }}</td>
                                <td class="text-left">{{ plataforma.tipoServico }}</td>
                                <td>
                                    <v-btn class="orange  lighten-1 white--text" @click="alterarPlataforma(plataforma)">
                                        <v-icon left>mdi-wrench </v-icon>
                                        Alterar
                                    </v-btn>
                                    <v-btn class="red lighten-1 white--text" @click="removerPlataforma(plataforma.id)">
                                        <v-icon dark left>mdi-cancel </v-icon>
                                        Remover
                                    </v-btn>
    
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
        name: "PlataformaForm",
        data: () => ({
            plataforma: {
            },
            listaPlataforma: [],
            indice: -1,
            items: ['Pago', "Grátis"],
            search: ""


        }),
        mounted(){
            this.getPlataforma();
        },
        methods: {
            // MÉTODO GET
            async getPlataforma(){
                const req = await fetch('http://localhost:3000/plataforma');
                const data = await req.json();
                this.listaPlataforma = data;
            },
            // MÉTODO SEARCH (PESQUISA)
            async searchPlataforma(){
                const req = await fetch('http://localhost:3000/plataforma?q='+this.search);
                const data = await req.json();
                this.listaPlataforma = data;
            },
            // LIMPA OS CAMPOS
            limpaPlataforma() {
                this.plataforma = {
                    id: 0,
                    plataforma: "",
                    tipoServico: ""
                };
                this.indice = -1;
            },
            // MÉTODO QUE VAI SALVAR E ATUALIZAR 
            async salvarPlataforma() {
                                
                if(this.indice < 0){
                    const objData = JSON.stringify(this.plataforma);
                    const req = await fetch('http://localhost:3000/plataforma',
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
                    
                    const objData = JSON.stringify(this.plataforma);
                    this.alterarPlataforma(this.plataforma);
                    const req = await fetch('http://localhost:3000/plataforma/'+ this.indice,
                    {   
                        method: "PUT",
                        body: objData,
                        headers: {
                            "Content-type": "application/json"
                        }
                    });
                    const data = await req.json();
                    this.listaPlataforma = data;
                }

                this.getPlataforma();
                this.limpaPlataforma();
            },
            // MÉTODO QUE VAI ALTERAR I INDICE PARA A EDIÇÃO DO VALOR
            async alterarPlataforma(plat){
                this.indice = plat.id;
                this.plataforma = plat;
            },
            // MÉTODO QUE VAI REMOVER O OBJETO
            async removerPlataforma(indice){
                const req = await fetch('http://localhost:3000/plataforma/'+indice,
                {
                    method: "DELETE",
                    headers: { 
                        "Content-type": "application/json"
                    }
                });
                const data = await req.json();
                console.log(data);
                this.getPlataforma();

            }
        }

    };
</script>

<style scoped>
    
</style>