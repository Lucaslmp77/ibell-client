<template>
    <div class="columnsCadastrar">
        <h1>REGISTRAR ENTRADA DE ESTOQUE</h1>
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
                    <select v-model ="stockInput.product">
                        <option value="undefined" disabled hidden>Selecione o Produto</option>
                        <option :value="item" 
                            v-for= "item in productList" :key="item.id"> {{ item.productName }} </option>
                    </select>
                </div>
            </div>
            <div class="control">
                <div class="select is-fullwidth">
                    <select v-model ="stockInput.provider">
                        <option value="undefined" disabled hidden>Selecione o Fornecedor</option>
                        <option :value="item" 
                            v-for= "item in providerList" :key="item.id"> {{ item.name }} </option>
                    </select>
                </div>
            </div>
        </div>
        <div class="field is-grouped">
            <div class="control">
                <input class="input" type="number" v-model="stockInput.costValue" placeholder="Valor de Custo">
            </div>
            <div class="control">
                <input class="input" type="number" v-model="stockInput.inputQuantity" placeholder="Quantidade de Entrada">
            </div>
        </div>
        <div class="field is-grouped">
            <div class="control">
                <textarea class="textarea" v-model="stockInput.observation" placeholder="Observação"></textarea>
            </div>
        </div>
        <div class="field is-grouped">
            <div class="control">
                <input class="input" type="datetime-local" v-model="stockInput.dateEntry" placeholder="Data de Entrada">
            </div>
        </div>
        <div class="field is-grouped">
            <div class="control">
                <router-link to="/stock-input"><button class="button is-link is-light">Voltar</button></router-link>
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
    }
</style>

<script lang="ts">
    import { ProductClient } from '@/client/Product.client'
    import { ProviderClient } from '@/client/Provider.client'
    import { StockInputClient } from '@/client/StockInput.client'
    import { Product } from '@/model/Product'
    import { Provider } from '@/model/Provider'
    import { StockInput } from '@/model/StockInput'
    import { Component, Vue } from 'vue-property-decorator'
    import {Mensagem} from "@/model/Mensagem";

    @Component
    export default class RegisterStockInputView extends Vue {

        private stockInputClient: StockInputClient = new StockInputClient()
        private productClient: ProductClient = new ProductClient()
        private providerClient: ProviderClient = new ProviderClient()

        public stockInput: StockInput = new StockInput()

        private notificacao: Mensagem = new Mensagem()

        public providerList: Provider[] = []
        public productList: Product[] = []

        public mounted(): void {
            this.selectProductList()
            this.selectProviderList()
        }

        public onClickCadastrar(): void {
            
            this.stockInputClient.save(this.stockInput).then(
                success => {
                    console.log('Registro Cadastrado com sucesso!!!')
                    this.notificacao = this.notificacao.new(
                        true, 'notification is-primary', 'Registrado com sucesso!'
                    )
                    this.stockInput = new StockInput()
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

        private selectProviderList(): void {
            this.providerClient.findByActiveProviders().then(
                success => this.providerList = success,
                error => console.log(error)
            )
        }

        private onClickFecharNotificacao(): void {
          this.notificacao = new Mensagem()
        }
    }
</script>