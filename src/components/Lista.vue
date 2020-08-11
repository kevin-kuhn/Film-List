<template>
  <div class="container-lista">
    <input type="text" class="input" v-model="novoFilme" @keyup.enter="adicionarFilme" />
    <div class="item-lista" v-for="filme of filmesFiltrados" :key="filme.id">
      <div class="primeiro-bloco">
        <input type="checkbox" v-model="filme.assistido" />
        <div
          v-if="!filme.editando"
          @dblclick="editarFilme(filme)"
          class="item-lista-label"
          :class="{ filmeAssistido : filme.assistido }"
        >{{ filme.nome }}</div>
        <input
          v-else
          v-focus
          class="edicao-item-lista"
          type="text"
          v-model="filme.nome"
          @blur="edicaoTerminada(filme)"
          @keyup.enter="edicaoTerminada(filme)"
          @keyup.esc="cancelarEdicao(filme)"
        />
      </div>
      <button @click="removerFilme(index)">&times;</button>
    </div>
    <div class="controle">
      <div class="check-div">
        <label>
          <input type="checkbox" :checked="!nenhumRestante" @change="marcarTodos" />Marcar todos
        </label>
      </div>
      <div>
        <button @click="filtroEscolhido ='todos'">Todos</button>
        <button @click="filtroEscolhido = 'naoAssistidos'">Não assistidos</button>
        <button @click="filtroEscolhido = 'assistidos'">Assistidos</button>
      </div>
      <div>
        {{ restantes }} filmes não assistidos
        <button v-if="mostrarBotaoDeLimpeza" @click="limparAssistidos">Limpar assistidos</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      novoFilme: "",
      idParaFilme: 2,
      nomeAntigo: "",
      filtroEscolhido: "todos",
      filmes: [
        { id: 1, nome: "o olhar dos deuses", assistido: false, editando: false }
      ]
    };
  },
  directives: {
    focus: {
      //nao funciona (custom directive para dar autoFoco ao input de edicao)
      inserted: function(el) {
        this.$nextTick(() => el.focus());
      }
    }
  },
  computed: {
    //criar nova data que depende de outras data (nao acieta parametro, nao altera outras date e sempre retorna algo)
    restantes() {
      return this.filmes.filter(filme => !filme.assistido).length;
    },
    nenhumRestante() {
      return this.restantes !== 0;
    },
    filmesFiltrados() {
      if (this.filtroEscolhido === "todos") {
        return this.filmes;
      }
      if (this.filtroEscolhido === "assistidos") {
        return this.filmes.filter(filme => filme.assistido === true);
      }
      return this.filmes.filter(filme => !filme.assistido === true);
    },
    mostrarBotaoDeLimpeza() {
      return this.filmes.filter(filme => filme.assistido === true).length > 0;
    }
  },
  methods: {
    adicionarFilme() {
      if (this.novoFilme.trim().length === 0) {
        return;
      }
      this.filmes.push({
        id: this.idParaFilme,
        nome: this.novoFilme,
        assistido: false,
        editando: false
      });

      this.idParaFilme++;
      this.novoFilme = "";
    },
    editarFilme(filme) {
      if (filme.nome.trim() === "") {
        filme.nome = this.nomeAntigo;
      }
      this.nomeAntigo = filme.nome;
      filme.editando = true;
    },
    removerFilme(index) {
      this.filmes.splice(index, 1);
    },
    edicaoTerminada(filme) {
      filme.editando = false;
    },
    cancelarEdicao(filme) {
      filme.nome = this.nomeAntigo;
      filme.editando = false;
    },
    marcarTodos() {
      this.filmes.forEach(filme => (filme.assistido = event.target.checked));
    },
    limparAssistidos() {
      this.filmes = this.filmes.filter(filme => !filme.assistido)
    }
  }
};
</script>

<style lang="less" scoped>
.input {
  height: 30px;
  margin-bottom: 10px;
  border-radius: 8px;
  border: 1px solid grey;
}
.container-lista {
  display: flex;
  width: 100%;
  height: 100%;
  flex-direction: column;
}
.item-lista {
  display: flex;
  justify-content: space-between;
  height: 30px;
  margin-bottom: 5px;
   &:hover{
     background: rgb(201, 207, 206, 0.3);
     border-radius: 8px;
   }
}
.filmeAssistido {
  text-decoration: line-through;
}
.primeiro-bloco {
  display: flex;
  align-items: center;
  justify-content: center;
}
.controle {
  display: flex;
  justify-content: space-between;
  margin-top: 10px;
  border-top: 1px solid grey;
}
.check-div{
  display: flex;
  align-items: center;
  justify-content: center;
}
.item-lista-label{
  display: flex;
  align-items: center;
  justify-content: center;

}
label {
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>