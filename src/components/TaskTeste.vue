<template>
    <div class="task-item">
        <span class="task-id">#{{ task?.id }}</span>
        <span class="task-title">{{ task?.title }}</span>
        <span class="task-status">{{ btntoggleStatus }}</span>
        <button class="task-complete" @click="toggleDone"  >{{btnToggleText}}</button>
        <button class="task-remove" @click="removeTask">Remover</button>
    </div>
</template>

<script>
export default {
    name: 'TaskItem',
    props: {
       task:{
         type: Object,
         required: true
       }    
    },
    methods: {
        toggleDone(){
            this.$emit('toggle-done', this.task.id);
        },
        removeTask(){
            this.$emit('remove-task', this.task.id);
        }
    },
    computed:{
        btnToggleText(){
            return this.task?.done ? 'Desfazer' : 'Concluir';
        },
        btntoggleStatus(){
            return this.task?.done ? 'pendente' : 'concluída';
        }
    }
};
</script>


<style>
/* Item da Tarefa */
.task-item {
    display: flex;
    align-items: center;
    justify-content: space-between; /* Garante que o texto fique de um lado e botões do outro */
    gap: 15px;
    padding: 12px 18px;
    border: 1px solid #e0e0e0;
    border-radius: 8px;
    margin-bottom: 12px;
    background-color: #fff;
    transition: box-shadow 0.2s ease;
}

.task-item:hover {
    box-shadow: 0 4px 8px rgba(0,0,0,0.05);
}

/* Informações da Tarefa (ID e Texto) */
.task-info {
    display: flex;
    align-items: center;
    gap: 10px;
    flex-grow: 1;
}

.task-id {
    font-weight: bold;
    color: #6c757d;
    font-size: 0.9em;
    background: #f1f3f5;
    padding: 2px 6px;
    border-radius: 4px;
}

.task-status {
    font-weight: 600;
    color: #28a745;
    font-size: 0.85em;
    text-transform: uppercase;
}

/* Grupo de Botões de Ação */
.task-actions {
    display: flex;
    gap: 8px;
}

/* Estilização Geral dos Botões de Tarefa */
.task-complete, .task-remove {
    border: none;
    padding: 8px 12px;
    border-radius: 6px;
    cursor: pointer;
    font-size: 0.85em;
    font-weight: 500;
    transition: opacity 0.2s, transform 0.1s;
}

.task-complete {
    background-color: #4CAF50;
    color: white;
}

.task-remove {
    background-color: #ef4444;
    color: white;
}

.task-complete:hover, .task-remove:hover {
    opacity: 0.9;
}

.task-complete:active, .task-remove:active {
    transform: scale(0.95);
}

</style>