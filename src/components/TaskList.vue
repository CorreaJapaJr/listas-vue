<template>
  <div class="container">
    <h1>Lista de Tarefas</h1>

    <div class="controls">
      <button 
      :class="btnaddClass"
      @click="handleShowForm">{{ btnAddtext }}</button>
      <button class="btn-clear" @click="clearTasks">Limpar Tarefas</button>
      <button class="btn-all" @click="markAllDone">Marcar Todas</button>
    </div>

    <div 
       v-if="showForm" 
    class="add-task-container">
      <input
        v-model="newTaskTitle"
        type="text"
        placeholder="Digite a nova tarefa..."
        class="task-input"
      />
      <button @click="AddTask" class="btn-add">Adicionar Tarefa</button>
    </div>

    <div class="task-container">
      <div class="pending-tasks">
        <h2>Tarefas Pendentes</h2>
        <p v-if="pendingTasks.length === 0">Nenhuma tarefa pendente</p>
        <div v-else>
        <TaskTeste
          v-for="task in pendingTasks"
          :key="task.id"
          :task="task"
          @toggle-done="toggleTaskDone"
          @remove-task="removeTask"
        />
        <span> Listas de tarefas pendentes vão aqui </span>
      </div>
      </div>
    </div>

    <div class="completed-task">
      <h2>Tarefas Concluídas</h2>
         <p v-if="completedTasks.length === 0">Nenhuma tarefa concluída</p>
         <div v-else>
      <TaskTeste
        v-for="task in completedTasks"
        :key="task.id"
        :task="task"
        @toggle-done="toggleTaskDone"
        @remove-task="removeTask"
      />
      <span> Listas de tarefas concluídas vão aqui </span>
    </div>
    </div>

        <div>
            <h3>Resumo</h3>
            <p v-if="tasks.length === 0">Você ainda não possui tarefas</p>
            <p v-else-if="pendingTasks.length > 0 && completedTasks.length === 0">Você tem {{ pendingTasks.length }} tarefas</p>
            <p v-else-if="completedTasks.length > 0 && pendingTasks.length === 0">Você tem todas as tarefas concluídas</p>
            <p v-else>Você tem {{ pendingTasks.length }} pendentes e {{ completedTasks.length }} concluídas</p>
        </div>

    <div class="watch-container">
      <h3>Saida do Watch</h3>
      <div class="log-container">
        {{ watchLogs }}
      </div>
    </div>
  </div>
</template>

<script>
  import TaskTeste from './TaskTeste.vue';

  export default {
    name: 'TaskList',
    components: {
      TaskTeste,
    },
    data() {
      return {
        tasks: [
          { id: 1, title: 'Tarefa 1', done: false },
          { id: 2, title: 'Tarefa 2', done: true },
          { id: 3, title: 'Tarefa 3', done: false },
          { id: 4, title: 'Tarefa 4', done: true },
        ],
        watchLogs: [],
        newTaskTitle: '',
        showForm: false,
      };
    },
    beforeCreate() {
      console.log('beforeCreate: Componente está sendo criado');
    },
    created() {
      console.log('created: Componente foi criado');
    },
    beforeMount() {
      console.log('beforeMount: Componente está prestes a ser montado');
    },
    mounted() {
      console.log('mounted: Componente foi montado no DOM');
    },
    beforeUpdate() {
      console.log('beforeUpdate: Componente está prestes a ser atualizado');
    },
    updated() {
      console.log('updated: Componente foi atualizado');
    },
    beforeUnmount() {
      console.log('beforeUnmount: Componente está prestes a ser destruído');
    },
    unmounted() {
      console.log('unmounted: Componente foi destruído');
    },
    methods: {
      removeTask(taskId) {
        this.tasks = this.tasks.filter(task => task.id !== taskId);
      },
      toggleTaskDone(taskId) {
        const task = this.tasks.find(t => t.id === taskId);
        if (task) {
          task.done = !task.done;
        }
      },
      logWatch(message) {
        this.watchLogs.unshift(
          `[${new Date().toLocaleTimeString()}] ${message}`
        );
      },
      AddTask() {
        if (this.newTaskTitle.trim() === '') return;

        this.tasks.push({
          id: Date.now(),
          title: this.newTaskTitle.trim(),
          done: false,
        });
        this.newTaskTitle = '';
        this.showForm = false;
      },
      handleShowForm() {
        this.showForm = !this.showForm;
      },
      clearTasks() {
        this.tasks = [];
      },
      markAllDone() {
        this.tasks.forEach(task => task.done = true);
      },
    },
    watch: {
      tasks: {
        handler(newVal, oldVal) {
          const message = `Tarefas atualizadas. Total: ${newVal.length}`;
          this.logWatch(message);
          if (oldVal) {
            const modified = newVal.filter(n => {
              const oldTask = oldVal.find(o => o.id === n.id);
              return oldTask && JSON.stringify(oldTask) !== JSON.stringify(n);
            });
            if (modified.length > 0) {
              const modifyMSg = `tarefas modificadas: ${modified
                .map(t => t.id)
                .join(', ')}`;
              this.logWatch(modifyMSg);
            }
          }
        },
        deep: true,
        immediate: true,
      },
    },
    computed: {
      completedTasks() {
        return this.tasks.filter(task => task.done);
      },
      pendingTasks() {
        return this.tasks.filter(task => !task.done);
      },
      btnAddtext(){
        return this.showForm ? 'Fechar' : 'Adicionar Tarefa';
      },
      btnaddClass(){
        return this.showForm ? 'btn-close' : 'btn-add';
    },
    },
  };    
</script>

<style>
/* =====================================================
   TEMA RETRÔ — TaskList
   Paleta: sépia, papiro, verde musgo, mogno
   ===================================================== */

/* Container principal — cartão de papel envelhecido */
.container {
  max-width: 860px;
  width: 100%;
  margin: 0 auto;
  padding: 36px 40px;
  background-color: #f5edda;
  background-image:
    url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='4' height='4'%3E%3Crect width='4' height='4' fill='%23f5edda'/%3E%3Crect x='0' y='0' width='1' height='1' fill='%23e8dcc4' opacity='0.4'/%3E%3C/svg%3E");
  border: 3px double #8b6914;
  border-radius: 2px;
  box-shadow:
    4px 4px 0 #7a5c10,
    8px 8px 0 rgba(0,0,0,0.15),
    inset 0 0 60px rgba(139, 105, 20, 0.08);
  position: relative;
}

/* Faixa decorativa no topo */
.container::before {
  content: '✦  LISTA DE TAREFAS  ✦';
  display: block;
  text-align: center;
  font-family: 'Special Elite', 'Courier New', monospace;
  font-size: 0.65rem;
  letter-spacing: 4px;
  color: #8b6914;
  border-bottom: 1px solid #c9a84c;
  padding-bottom: 10px;
  margin-bottom: 24px;
  opacity: 0.7;
}

/* Título principal */
.container h1 {
  font-family: 'Special Elite', 'Courier New', monospace;
  font-size: 2rem;
  color: #3b2a09;
  text-align: center;
  margin: 0 0 28px 0;
  letter-spacing: 2px;
  text-shadow: 1px 1px 0 rgba(255,255,255,0.5);
  border-bottom: 2px solid #c9a84c;
  padding-bottom: 16px;
}

/* Seção de resumo */
.container > div h3 {
  font-family: 'Special Elite', 'Courier New', monospace;
  color: #5c3d11;
  font-size: 1rem;
  letter-spacing: 1px;
  margin-bottom: 6px;
  text-transform: uppercase;
}

.container > div > p {
  font-family: 'Courier Prime', 'Courier New', monospace;
  color: #6b4e1a;
  font-size: 0.9rem;
  margin: 4px 0;
}

/* ---- Controles / Barra de Ações ---- */
.controls {
  display: flex;
  gap: 10px;
  margin-bottom: 22px;
  flex-wrap: wrap;
}

/* ---- Formulário de Adicionar ---- */
.add-task-container {
  display: flex;
  gap: 12px;
  margin-bottom: 26px;
}

.task-input {
  flex: 1;
  padding: 10px 14px;
  font-family: 'Courier Prime', 'Courier New', monospace;
  font-size: 0.95rem;
  color: #3b2a09;
  background-color: #fdf6e3;
  border: 2px solid #c9a84c;
  border-radius: 2px;
  outline: none;
  box-shadow: inset 2px 2px 4px rgba(139, 105, 20, 0.12);
  transition: border-color 0.25s, box-shadow 0.25s;
}

.task-input::placeholder {
  color: #b09060;
  font-style: italic;
}

.task-input:focus {
  border-color: #8b6914;
  box-shadow: inset 2px 2px 4px rgba(139, 105, 20, 0.2), 0 0 0 3px rgba(201, 168, 76, 0.2);
}

/* ---- Botões — estilo carimbo/relevo retrô ---- */
.btn-add, .btn-clear, .btn-all, .btn-close {
  font-family: 'Special Elite', 'Courier New', monospace;
  font-size: 0.82rem;
  letter-spacing: 1.5px;
  text-transform: uppercase;
  padding: 9px 18px;
  border-radius: 2px;
  cursor: pointer;
  transition: transform 0.1s, box-shadow 0.1s, filter 0.15s;
  position: relative;
}

/* Verde musgo — Adicionar */
.btn-add {
  background-color: #4a7c59;
  color: #f0ead2;
  border: 2px solid #2e5c3a;
  box-shadow: 3px 3px 0 #1e3d26;
}

/* Mogno escuro — Limpar */
.btn-clear {
  background-color: #8b3a2f;
  color: #f0ead2;
  border: 2px solid #5c2218;
  box-shadow: 3px 3px 0 #3a1510;
}

/* Azul ardósia — Marcar Todas */
.btn-all {
  background-color: #4a6580;
  color: #f0ead2;
  border: 2px solid #2e4a5c;
  box-shadow: 3px 3px 0 #1e3040;
}

/* Cinza sépia — Fechar */
.btn-close {
  background-color: #7a6a4f;
  color: #f0ead2;
  border: 2px solid #5c4e38;
  box-shadow: 3px 3px 0 #3d3425;
}

.btn-add:hover, .btn-clear:hover, .btn-all:hover, .btn-close:hover {
  filter: brightness(1.1);
}

.btn-add:active, .btn-clear:active, .btn-all:active, .btn-close:active {
  transform: translate(3px, 3px);
  box-shadow: 0 0 0 transparent;
}

/* ---- Colunas de Tarefas ---- */
.task-container {
  display: flex;
  gap: 20px;
  margin-bottom: 28px;
}

.pending-tasks, .completed-task {
  flex: 1;
  background-color: #fdf6e3;
  border: 1px solid #c9a84c;
  border-radius: 2px;
  padding: 16px;
  min-height: 200px;
  box-shadow: inset 0 1px 4px rgba(139, 105, 20, 0.1);
}

.pending-tasks h2, .completed-task h2 {
  font-family: 'Special Elite', 'Courier New', monospace;
  font-size: 0.95rem;
  text-transform: uppercase;
  letter-spacing: 2px;
  color: #5c3d11;
  margin-top: 0;
  padding-bottom: 10px;
  margin-bottom: 14px;
  text-align: center;
  border-bottom: 2px dashed #c9a84c;
}

.pending-tasks > p, .completed-task > p {
  font-family: 'Courier Prime', 'Courier New', monospace;
  font-style: italic;
  color: #a08050;
  font-size: 0.88rem;
  text-align: center;
  margin-top: 30px;
}

/* ---- Terminal DOS — Logs do Watch ---- */
.watch-container {
  background-color: #0d0d0d;
  border: 2px solid #3a3a3a;
  border-radius: 2px;
  padding: 0;
  margin-top: 28px;
  box-shadow: 4px 4px 0 #000, inset 0 0 30px rgba(0,0,0,0.5);
  overflow: hidden;
}

/* Barra de título de janela DOS */
.watch-container h3 {
  font-family: 'VT323', 'Courier New', monospace;
  font-size: 1rem;
  color: #000;
  background-color: #aaaaaa;
  margin: 0;
  padding: 4px 10px;
  text-transform: uppercase;
  letter-spacing: 1px;
  border-bottom: 2px solid #555;
  display: flex;
  align-items: center;
  gap: 8px;
}

.watch-container h3::before {
  content: '■ □ □';
  font-size: 0.7rem;
  letter-spacing: 3px;
}

.log-container {
  max-height: 180px;
  overflow-y: auto;
  font-family: 'VT323', 'Courier New', monospace;
  font-size: 1.1rem;
  line-height: 1.5;
  color: #33ff33;
  padding: 12px 14px;
  white-space: pre-wrap;
  text-shadow: 0 0 8px rgba(51, 255, 51, 0.6);
}

/* Cursor piscante estilo terminal */
.log-container::after {
  content: '█';
  animation: blink 1s step-end infinite;
  color: #33ff33;
}

@keyframes blink {
  0%, 100% { opacity: 1; }
  50% { opacity: 0; }
}

/* Scrollbar estilo terminal */
.log-container::-webkit-scrollbar { width: 8px; }
.log-container::-webkit-scrollbar-track { background: #111; }
.log-container::-webkit-scrollbar-thumb {
  background: #33ff33;
  border-radius: 0;
}

/* Span de placeholder dentro das listas */
.task-container span,
.completed-task span {
  font-family: 'Courier Prime', 'Courier New', monospace;
  font-size: 0.8rem;
  color: #b09060;
  font-style: italic;
  display: block;
  text-align: center;
  margin-top: 8px;
}
</style>

