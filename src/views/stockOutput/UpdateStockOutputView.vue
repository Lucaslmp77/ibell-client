<template>
    <div class="columnsCadastrar">
        <h1>EDITAR SAÍDA DE ESTOQUE</h1>
        <div class="columns" v-if="notificacao.ativo">
          <div class="column is-12">
            <div :class="notificacao.classe">
              <button @click="onClickFecharNotificacao()" class="delete" ></button>
              {{ notificacao.mensagem }}
            </div>
          </div>
        </div>
        <div class="field is-grouped">
            <div class="control">
                <div class="select is-fullwidth">
                    <select v-model ="stockOutput.product">
                        <option value="undefined" disabled hidden>Selecione o Produto</option>
                        <option :value="item" 
                            v-for= "item in productList" :key="item.id"> {{ item.productName }} </option>
                    </select>
                </div>
            </div>
            <div class="control">
                <div class="select is-fullwidth">
                    <select v-model ="stockOutput.client">
                        <option value="undefined" disabled hidden>Selecione o Cliente</option>
                        <option :value="item" 
                            v-for= "item in clientList" :key="item.id"> {{ item.name }} </option>
                    </select>
                </div>
            </div>
        </div>
        <!-- <div class="field is-grouped">
            <div class="control">
                <label>Valor de Saida</label>
                <input class="input" type="number" v-model="stockOutput.saleValue" placeholder="Valor de Saida">
            </div>
            <div class="control">
                <label>Quantidade de Saída</label>
                <input class="input" type="number" v-model="stockOutput.quantityOutput" placeholder="Quantidade de Saída">
            </div>
        </div> -->
        <div class="field is-grouped">
            <div class="control">
                <label>Observação</label>
                <textarea class="textarea" v-model="stockOutput.observation" placeholder="Observação"></textarea>
            </div>
        </div>
        <div class="field is-grouped">
            <div class="control">
                <label>Data de Saída</label>
                <input class="input" type="datetime-local" v-model="stockOutput.departureDate" placeholder="Data de Saída">
            </div>
        </div>
        <div class="field is-grouped">
            <div class="control">
                <router-link to="/stock-output"><button class="button is-link is-light">Voltar</button></router-link>
            </div>
            <div class="control">
                <button @click="onClickCadastrar()" class="button is-success is-focused">Salvar</button>
            </div>
        </div>
    </div>
</template>

<style scoped lang="scss">
    .columnsCadastrar {
        display: flex;
        align-items: center;
        justify-content: center;
        flex-direction: column;
        gap: 15px;

        h1 {
            font-size: 36px;
            color: black;
            font-weight: bold;
            margin-bottom: 15px;
        }

        .textarea {
            width: 400px;
            max-height: 200px;
        }

        label {
            color: black;
            font-weight: 700;
        }
    }
</style>

<script lang="ts">
    import { ClientClient } from '@/client/Client.client'
    import { ProductClient } from '@/client/Product.client'
    import { StockOutputClient } from '@/client/StockOutput.client'
    import { Client } from '@/model/Client'
    import { Product } from '@/model/Product'
    import { StockOutput } from '@/model/StockOutput'
    import { Component, Vue } from 'vue-property-decorator'
    import {Mensagem} from "@/model/Mensagem";

    @Component
    export default class UpdateStockOutputView extends Vue {

        private stockOutputClient: StockOutputClient = new StockOutputClient()
        private productClient: ProductClient = new ProductClient()
        private clientClient: ClientClient = new ClientClient()
        private notificacao: Mensagem = new Mensagem()

        public stockOutput: StockOutput = new StockOutput()

        public clientList: Client[] = []
        public productList: Product[] = []

        private id = Number(this.$route.params.id)

        public mounted(): void {
            this.selectProductList()
            this.selectClientList()
            this.getStockOutput()
        }

        private getStockOutput(): void {
            this.stockOutputClient.findById(this.id).then(
                success => {
                    this.stockOutput = success
                },
                error => console.log(error)
            )
        }

        public onClickCadastrar(): void {
            
            this.stockOutputClient.update(this.stockOutput).then(
                success => {
                    console.log('Registro Cadastrado com sucesso!!!')
                    this.notificacao = this.notificacao.new(
                        true, 'notification is-primary', 'Atualização registrada com sucesso!'
                    )
                    this.stockOutput = new StockOutput()
                },
                error => {
                    console.log(error)
                }
            )
        }

        private selectProductList(): void {
            this.productClient.findByActiveProducts().then(
                success => this.productList = success,
                error => console.log(error)
            )
        }

        private selectClientList(): void {
            this.clientClient.findByActiveClients().then(
                success => this.clientList = success,
                error => console.log(error)
            )
        }

        private onClickFecharNotificacao(): void {
          this.notificacao = new Mensagem()
        }
    }
</script>