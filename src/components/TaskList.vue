<template>
  <div class="container">
    <h1>Lista de Tarefas</h1>

    <div class="controls">
      <button 
      :class="btnaddClass"
      @click="handleShowForm">{{ btnAddtext }}</button>
      <button class="btn-clear">Limpar Tarefas</button>
      <button class="btn-all">Marcar Todas</button>
    </div>

    <div 
       v-if="showForm" 
    class="add-task-container">
      <input
        v-model="newTaskTitle"
        type="text"
        placeholder="Buscar tarefas..."
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
            <p v-if="tasks.length === 0">Voce ainda não possui tarefas</p>
            <p v-else-if="pendingTasks.length > 0 && completedTasks.length === 0">Voce tem {{ pendingTasks.length }} tarefas</p>
            <p v-else-if="completedTasks.length > 0 && pendingTasks.length === 0">voce tem todas as tarefas concluidas</p>
            <p v-else>Voce tem {{ pendingTasks.length }} pendetes e {{ completedTasks.length }} concluidas </p>
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
/* Configurações Gerais */
body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-color: #e9ecef;
  display: flex;
  justify-content: center;
  padding: 40px;
}

.container {
  max-width: 850px;
  width: 100%;
  margin: 0 auto;
  padding: 30px;
  border-radius: 12px;
  background-color: #ffffff;
  box-shadow: 0 10px 25px rgba(0,0,0,0.1);
}

/* Área de Input e Botões Principais */
.add-task-container {
  display: flex;
  gap: 12px;
  margin-bottom: 25px;
}

.task-input {
  flex: 1;
  padding: 12px 15px;
  border: 2px solid #dee2e6;
  border-radius: 8px;
  outline: none;
  transition: border-color 0.3s;
}

.task-input:focus {
  border-color: #007bff;
}

.controls {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

/* Botões */
.btn-add, .btn-clear, .btn-all, .btn-close {
  padding: 10px 18px;
  border: none;
  border-radius: 6px;
  font-weight: 600;
  cursor: pointer;
  transition: filter 0.2s, transform 0.1s;
}

.btn-add { background-color: #28a745; color: white; }
.btn-clear { background-color: #dc3545; color: white; }
.btn-all { background-color: #007bff; color: white; }
.btn-close { background-color: #6c757d; color: white; }

.btn-add:hover, .btn-clear:hover, .btn-all:hover {
  filter: brightness(90%);
}

.btn-add:active { transform: scale(0.98); }

/* Colunas de Tarefas */
.task-container {
  display: flex;
  gap: 20px;
  margin-bottom: 30px;
}

.pending-tasks, .completed-task {
  flex: 1;
  border: 1px solid #edf2f7;
  border-radius: 10px;
  padding: 15px;
  background-color: #f8f9fa;
  min-height: 200px;
}

.pending-tasks h2, .completed-task h2 {
  font-size: 1.1rem;
  color: #4a5568;
  margin-top: 0;
  border-bottom: 2px solid #dee2e6;
  padding-bottom: 10px;
  margin-bottom: 15px;
  text-align: center;
}

/* Área de Logs/Console (Watch) */
.watch-container {
  background-color: #1e293b;
  border-radius: 8px;
  padding: 15px;
  margin-top: 20px;
}

.log-container {
  max-height: 200px;
  overflow-y: auto;
  font-family: 'Fira Code', 'Courier New', monospace;
  font-size: 13px;
  line-height: 1.6;
  color: #38bdf8; /* Azul neon suave para leitura */
}

/* Scrollbar Personalizada para o Log */
.log-container::-webkit-scrollbar {
  width: 6px;
}
.log-container::-webkit-scrollbar-thumb {
  background: #475569;
  border-radius: 10px;
}

</style>
