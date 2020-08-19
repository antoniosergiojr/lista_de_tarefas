var app = new Vue({
  el: "#listaTarefasApp",
  data: {
    titulo: "Lista de Tarefas",
    tarefa: "",
    tarefas: [],
  },
  computed: {
    tarefasOrdenadasPorFinalizadas: function () {
      return _.orderBy(this.tarefas, ["finalizada", false]);
    },
  },
  methods: {
    adicionarTarefa: function () {
      this.tarefas.push({
        descricao: this.tarefa,
        finalizada: false,
      });
      localStorage.setItem("tarefas", JSON.stringify(this.tarefas));
      this.tarefa = "";
    },
    finalizarTarefa: function (tarefa) {
      tarefa.finalizada = !tarefa.finalizada;
      localStorage.setItem("tarefas", JSON.stringify(this.tarefas));
    },
    removerTarefa: function (tarefa) {
      if (confirm("Tem certeza que deseja deletar essa tarefa?")) {
        var id = _.findIndex(this.tarefas, tarefa);
        this.tarefas.splice(id, 1);
        localStorage.setItem("tarefas", JSON.stringify(this.tarefas));
      }
    },
    removerTodasTarefas: function () {
      if (confirm("Tem certeza que deseja deletar todas as tarefas?")) {
        this.tarefas = [];
        localStorage.clear();
      }
    },
  },
  mounted() {
    if (localStorage.getItem("tarefas") != null) {
      this.tarefas = JSON.parse(localStorage.getItem("tarefas"));
    }
  },
});
