<template>
    <div class="columns is-fullwidth">
        <h1>PRODUTOS INATIVOS</h1>
        <div class="column">
            <p class="control">
                <input class="input search" type="text" placeholder="Pesquise aqui...">
            </p>
            <p class="control">
                <button class="button is-link">
                    Buscar
                </button>
            </p>
            <router-link to="/">
                <button class="button is-primary is-focused">
                    Voltar
                </button>
            </router-link>    
        </div>
        <table class="table is-bordered is-fullwidth">
            <thead>
                <tr>
                    <th>Data</th>
                    <th>Nome</th>
                    <th>Codigo</th>
                    <th>Unidade de Medida</th>
                    <th>Quantidade</th>
                    <th>Valor Unitario</th>
                    <th>Fornecedor</th>
                    <th>Observação</th>
                    <th>Opções</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for= 'item in productList'>
                    <th> {{ item.register }} </th>
                    <th> {{ item.productName }} </th>
                    <th> {{ item.code }} </th>
                    <th> {{ item.unitMeasure }} </th>
                    <th> {{ item.quantity }} </th>
                    <th> {{ item.unitValue }} </th>
                    <th> {{ item.provider.name }} </th>
                    <th> {{ item.observation }} </th>
                    <th class="opcoes">
                        <button @click="onClickEnabled(item.id)" class="button is-success">Ativar</button>
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

        .search {
            width: 800px;
        }

        .opcoes {
            display: flex;
            align-items: center;
            gap: 20px;
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
    import { Component, Vue } from 'vue-property-decorator';
    import { RouterLink } from "vue-router";
    import { ProductClient } from '@/client/Product.client';
    import { Product } from '@/model/Product';
    import router from '@/router';
    
    @Component
    export default class ProductsInactiveView extends Vue {

        private productClient: ProductClient = new ProductClient()

        public productList: Product[] = []

        public product: Product = new Product()

        public mounted(): void{
            this.listProdutos()
        }

        private listProdutos(): void{
            this.productClient.findByInactiveProducts().then(
                success => {
                    this.productList = success
                },
                error => {
                    console.log(error)
                }
            )
        }

        public onClickPageUpdate(id: number) {
            router.push({ path:`/update-product/${id}` })
        }

        public onClickEnabled(id: number) {
            this.productClient.enabled(id).then(
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