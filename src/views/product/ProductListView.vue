<template>
    <div class="columns is-fullwidth">
        <h1>LISTA DE PRODUTOS</h1>
        <div class="column">
            <p class="control">
                <input class="input" type="text" placeholder="Pesquise aqui...">
            </p>
            <p class="control">
                <button class="button is-link">
                    Buscar
                </button>
            </p>
            <router-link to="/cadastrar-frete">
                <button class="button is-primary is-focused">
                    Cadastrar Produto
                </button>
            </router-link>    
        </div>
    
        <table class="table is-bordered is-fullwidth">
            <thead>
                <tr>
                    <th>Nome</th>
                    <th>Codigo</th>
                    <th>Unidade de Medida</th>
                    <th>Quantidade</th>
                    <th>Valor Unitario</th>
                    <th>Porcentagem</th>
                    <th>Fornecedor</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for= 'item in freteList'>
                    <th> {{ item.cadastrar }} </th>
                    <th> 
                        
                        <span v-if="item.statusFrete === 'CARGA'" class="tag is-info"> Em Carga </span>
                        <span v-if="item.statusFrete === 'EM_TRANSPORTE'" class="tag is-info"> Em Transporte </span>
                        <span v-if="item.statusFrete === 'INTERROMPIDO'" class="tag is-warning"> Interrompido </span>
                        <span v-if="item.statusFrete === 'DESCARGA'" class="tag is-primary"> Em Descarga </span>
                        <span v-if="item.statusFrete === 'FATURADO'" class="tag is-success"> Faturado </span>
                        <span v-if="item.statusFrete === 'CANCELADO'" class="tag is-danger"> Cancelado </span>

                    </th>
                    <th> {{ item.cidadeOrigem.nome }} </th>
                    <th> {{ item.cidadeDestino.nome }} </th>
                    <th> {{ item.caminhao.placa }} </th>
                    <th> {{ item.produto.nome }} </th>
                    <th>

                    </th>
                    <!-- @click="onClickPaginaDetalhar(item.id)" -->
                    <!-- <th><button class="button is-warning is-focused">Detalhar</button></th> -->
                </tr>
            </tbody>
        </table>
    </div>
</template>

<style scoped lang="scss">
    .columns {
        h1 {
            font-size: 36px;
            color: black;
        }
        .column {
            display: flex;
            align-items: center;
            gap: 10px;
            padding: 10px;
            .input {
                border-color: blue;
            }
        }
        display: flex;
        align-items: center;
        justify-content: center;
        flex-direction: column;
        gap: 20px;
        padding: 15px;
    }
</style>

<script lang="ts">
    import { Component, Vue } from 'vue-property-decorator'

    @Component
    export default class ProductListView extends Vue {

    }

</script>