<template>
  <div id="app">
    <nav class="navbar" role="navigation" aria-label="main navigation">
      <div class="navbar-brand">
        <a class="navbar-item">
          <h1>IBELL</h1>
        </a>
        <a role="button" class="navbar-burger" aria-label="menu" aria-expanded="false" data-target="navbarBasicExample">
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
        </a>
      </div>
      <div id="navbarBasicExample" class="navbar-menu">
        <div class="navbar-start">
          <router-link to ='/'> 
            Produtos
          </router-link> 
          <router-link to ='/client'> 
            Clientes
          </router-link>
          <router-link to ='/provider'> 
            Fornecedores
          </router-link> 
          <router-link to ='/stock-input'> 
            Entrada de Estoque
          </router-link> 
          <router-link to ='/stock-output'> 
            Saida de Estoque
          </router-link>
        </div>  
      </div>
    </nav>
    <router-view/>
  </div>
</template>

<style lang="scss">
  @import "~bulma/bulma.sass";

  #app {
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
  }

  nav {
    padding: 25px 30px;

    h1 {
      font-size: 56px;
      font-weight: 800;
      color: black;
    }

    .navbar-start {
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 0 40px;
      gap: 20px;
      font-size: 18px;
    }

    a {
        font-weight: bold;
        color: #2c3e50;
        &.router-link-exact-active {
        color: #42b983;
      }
    }
  }
</style>