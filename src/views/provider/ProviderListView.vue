<template>
    <div class="columns is-fullwidth">
        <h1>FORNECEDORES</h1>
        <div class="column">
            <p class="control">
                <input class="input search" type="text" placeholder="Pesquise aqui...">
            </p>
            <p class="control">
                <button class="button is-link">
                    Buscar
                </button>
            </p>
            <router-link to="/register-provider">
                <button class="button is-primary is-focused">
                    Cadastrar Fornecedor
                </button>
            </router-link>  
            <router-link to="/providers-inactives">
                <button class="button is-danger">
                    Visualizar Fornecedores Inativos
                </button>
            </router-link>    
        </div>
        <table class="table is-bordered is-fullwidth">
            <thead>
                <tr>
                    <th>Data</th>
                    <th>Nome</th>
                    <th>CNPJ/CPF</th>
                    <th>Telefone</th>
                    <th>Endereço</th>
                    <th>Email</th>
                    <th>Observação</th>
                    <th>Opções</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for= 'item in providerList'>
                    <th> {{ item.register }} </th>
                    <th> {{ item.name }} </th>
                    <th> {{ item.cnpjCpf }} </th>
                    <th> {{ item.phoneNumber }} </th>
                    <th> {{ item.addres }} </th>
                    <th> {{ item.email }} </th>
                    <th> {{ item.observation }} </th>
                    <th class="opcoes">
                        <button @click="onClickPageUpdate(item.id)" class="button is-warning is-focused">Editar</button>
                        <button @click="onClickDisable(item.id)" class="button is-danger">Desativar</button>
                    </th>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<style scoped lang="scss">
    .columns {
        h1 {
            font-size: 36px;
            font-weight: bold;
            color: black;
        }

        .column {
            display: flex;
            align-items: center;
            gap: 10px;
            padding: 0px 10px;
            .input {
                border-color: blue;
            }
        }

        table {
            thead {
                tr {
                    th {
                        font-size: 18px;
                    }
                }
            }
            tbody {
                tr {
                    th {
                        font-weight: 600;
                    }
                }
            }
        }

        .opcoes {
            display: flex;
            align-items: center;
            gap: 20px;
        }

        .search {
            width: 800px;
        }

        display: flex;
        align-items: center;
        justify-content: center;
        flex-direction: column;
        gap: 20px;
        padding: 0px 50px;
    }
</style>

<script lang="ts">
    import { ProviderClient } from '@/client/Provider.client';
    import { Provider } from '@/model/Provider';
    import router from '@/router';
    import { Component, Vue } from 'vue-property-decorator';
    import { RouterLink } from 'vue-router';
    
    @Component
    export default class ProviderListView extends Vue {
        private providerClient: ProviderClient = new ProviderClient()

        public providerList: Provider[] = []
        
        public provider: Provider = new Provider()

        public mounted(): void{
            this.listProviders()
        }

        private listProviders(): void{
            this.providerClient.findByActiveProviders().then(
                success => {
                    this.providerList = success
                },
                error => {
                    console.log(error)
                }
            )
        }

        public onClickPageUpdate(id: number) {
            router.push({ path:`/update-provider/${id}` })
        }

        public onClickDisable(id: number) {
            this.providerClient.disable(id).then(
                success => {
                    console.log("desativado com sucesso!!!")
                    window.location.reload()
                },
                error => {
                    console.log(error)
                }
            )
        }
    }
</script>