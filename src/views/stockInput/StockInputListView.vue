<template>
    <div class="columns is-fullwidth">
        <h1>ENTRADAS DE ESTOQUE</h1>
        <div class="columns" v-if="notificacao.ativo">
          <div class="column is-12">
            <div :class="notificacao.classe">
              <button @click="onClickFecharNotificacao()" class="delete" ></button>
              {{ notificacao.mensagem }}
            </div>
          </div>
        </div>
        <div class="column">
            <p class="control">
                <input class="input search" type="text" placeholder="Pesquise aqui...">
            </p>
            <p class="control">
                <button class="button is-link">
                    Buscar
                </button>
            </p>
            <router-link to="/register-stock-input">
                <button class="button is-primary is-focused">
                    Registrar Nova Entrada
                </button>
            </router-link>    
        </div>
        <table class="table is-bordered is-fullwidth">
            <thead>
                <tr>
                    <th>Data</th>
                    <th>Produto</th>
                    <th>Fornecedor</th>
                    <th>Valor de Custo</th>
                    <th>Quantidade de Entrada</th>
                    <th>Data de Entrada</th>
                    <th>Observação</th>
                    <th>Opções</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for= 'item in stockInputList'>
                    <th> {{ item.register }} </th>
                    <th> {{ item.product.productName }} </th>
                    <th> {{ item.provider.name }} </th>
                    <th> {{ item.costValue }} </th>
                    <th> {{ item.inputQuantity }} </th>
                    <th> {{ formatDate(item.dateEntry) }} </th>
                    <th> {{ item.observation }} </th>
                    <th><button @click="onClickPageUpdate(item.id)" class="button is-warning is-focused">Editar</button></th>
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
            width: 900px;
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
    import { StockInputClient } from '@/client/StockInput.client';
    import { StockInput } from '@/model/StockInput';
    import router from '@/router';
    import { Component, Vue } from 'vue-property-decorator';
    import { RouterLink } from 'vue-router';
    import {Mensagem} from "@/model/Mensagem";
    
    @Component
    export default class StockInputListView extends Vue {
        private stockInputClient: StockInputClient = new StockInputClient()

        public stockInputList: StockInput[] = []
        
        public stockInput: StockInput = new StockInput()

        private notificacao: Mensagem = new Mensagem()

        public mounted(): void{
            this.listStockInputs()
        }

        public formatDate(date : any) {
            let obj = new Date(date)
            return obj.toLocaleString()
        }

        private listStockInputs(): void{
            this.stockInputClient.findByActiveStockInputs().then(
                success => {
                    this.stockInputList = success
                },
                error => {
                    console.log(error)
                }
            )
        }

        public onClickPageUpdate(id: number) {
            router.push({ path:`/update-stock-input/${id}` })
        }
    }
</script>