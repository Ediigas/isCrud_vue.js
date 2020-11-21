<template>
  <div id="app" class="container">
    <h1>Distribuidora Piemonte</h1>
    <b-alert
      show
      dismissible
      v-for="mensagem in mensagens"
      :key="mensagem.texto"
      :variant="mensagem.tipo"
      >{{ mensagem.texto }}</b-alert
    >
    <b-card>
      <b-form-group>
        <b-form-input
          type="text"
          size="lg"
          v-model="cliente.empresa"
          placeholder="Cliente: "
        >
        </b-form-input>
      </b-form-group>
      <b-form-group>
        <b-form-input
          type="text"
          size="lg"
          v-model="cliente.cnpj"
          placeholder="CNPJ: "
        >
        </b-form-input>
      </b-form-group>
      <b-form-group>
        <b-form-input
          type="text"
          size="lg"
          v-model="cliente.rSocial"
          placeholder="Razão Social: "
        >
        </b-form-input>
      </b-form-group>
      <b-form-group>
        <b-form-input
          type="text"
          size="lg"
          v-model="cliente.endereco"
          placeholder="Endereço: "
        >
        </b-form-input>
      </b-form-group>
      <hr />
      <b-button @click="salvar" size="lg" variant="primary">Salvar</b-button>
      <b-button @click="obterClientes" size="lg" variant="success" class="ml-2"
        >Obter Clientes</b-button
      >
    </b-card>
    <hr />
    <b-list-group>
      <b-list-group-item v-for="(cliente, id) in clientes" :key="id">
        <strong>Cliente: </strong>{{ cliente.empresa }}<br />
        <strong>CNPJ: </strong>{{ cliente.cnpj }}<br />
        <strong>Razão Social: </strong>{{ cliente.rSocial }}<br />
        <strong>Endereço: </strong>{{ cliente.endereco }}<br />
        <strong>id: </strong>{{ id }}<br />
        <b-button variant="warning" size="lg" @click="carregar(id)"
          >Editar</b-button
        >
        <b-button variant="danger" size="lg" class="ml-2" @click="excluir(id)"
          >Excluir</b-button
        >
      </b-list-group-item>
    </b-list-group>
  </div>
</template>

<script>
export default {
  data() {
    return {
      mensagens: [],
      clientes: [],
      id: null,
      cliente: {
        empresa: "",
        cnpj: "",
        rSocial: "",
        endereco: "",
      },
    };
  },
  methods: {
    limpar() {
      this.cliente.empresa = "";
      this.cliente.cnpj = "";
      this.cliente.rSocial = "";
      this.cliente.endereco = "";
      this.id = null;
      this.mensagens = [];
    },

    carregar(id) {
      this.id = id;
      this.cliente = { ...this.clientes[id] };
    },

    excluir(id) {
      this.$http
        .delete(`/clientes/${id}.json`)
        .then(() => {
          this.limpar();
          this.obterUsuarios();
        })
        .catch(() => {
          this.mensagens.push({
            texto: "Excluido com sucesso! Recarrgue ",
            tipo: "success",
          });
        });
    },

    salvar() {
      const metodo = this.id ? "patch" : "post";
      const finalUrl = this.id ? `/${this.id}.json` : ".json";
      this.$http[metodo](`/clientes${finalUrl}`, this.cliente).then(() => {
        this.limpar();
        this.mensagens.push({
          texto: "Operação realiazada com sucesso!",
          tipo: "success",
        });
      });
    },

    obterClientes() {
      this.$http.get("clientes.json").then((res) => {
        this.clientes = res.data;
      });
    },
  },

};
</script>

<style>
#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin-top: 60px;
}

.container h1 {
  text-align: center;
  margin-bottom: 30px;
  color: green;
}
</style>
