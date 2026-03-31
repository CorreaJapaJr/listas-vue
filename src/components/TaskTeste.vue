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
/* =====================================================
   TEMA RETRÔ — TaskTeste (Card de Tarefa)
   Visual: ficha datilografada, papel envelhecido
   ===================================================== */

.task-item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 12px;
    padding: 10px 14px;
    background-color: #fdf6e3;
    border: 1px solid #c9a84c;
    border-left: 4px solid #8b6914;
    border-radius: 2px;
    margin-bottom: 10px;
    transition: transform 0.15s ease, box-shadow 0.15s ease;
    position: relative;
}

/* Linha perfurada no topo — papel de fichário */
.task-item::before {
    content: '';
    position: absolute;
    top: -1px;
    left: 20px;
    right: 20px;
    height: 1px;
    background: repeating-linear-gradient(
        to right,
        transparent,
        transparent 4px,
        #c9a84c55 4px,
        #c9a84c55 8px
    );
}

.task-item:hover {
    transform: translateX(3px);
    box-shadow: -3px 0 0 #8b6914, 3px 3px 0 rgba(139, 105, 20, 0.2);
}

/* Badge de ID — estilo carimbo numerado */
.task-id {
    font-family: 'VT323', 'Courier New', monospace;
    font-size: 1rem;
    font-weight: normal;
    color: #fff;
    background-color: #8b6914;
    padding: 2px 8px;
    border-radius: 2px;
    letter-spacing: 1px;
    flex-shrink: 0;
    box-shadow: 1px 1px 0 #5c4a0c;
}

/* Título da tarefa */
.task-title {
    font-family: 'Courier Prime', 'Courier New', monospace;
    font-size: 0.95rem;
    color: #3b2a09;
    flex: 1;
    letter-spacing: 0.3px;
}

/* Status da tarefa — badge colorido */
.task-status {
    font-family: 'Special Elite', 'Courier New', monospace;
    font-size: 0.7rem;
    text-transform: uppercase;
    letter-spacing: 1.5px;
    padding: 2px 7px;
    border-radius: 2px;
    border: 1px solid currentColor;
    flex-shrink: 0;
    /* cores definidas por classe pai (done/pendente) */
    color: #4a7c59;
    border-color: #4a7c59;
    background-color: rgba(74, 124, 89, 0.1);
}

/* Botões de ação — carimbo retrô pequeno */
.task-complete, .task-remove {
    font-family: 'Special Elite', 'Courier New', monospace;
    font-size: 0.72rem;
    letter-spacing: 1px;
    text-transform: uppercase;
    padding: 6px 12px;
    border-radius: 2px;
    cursor: pointer;
    flex-shrink: 0;
    transition: transform 0.1s, box-shadow 0.1s, filter 0.15s;
}

.task-complete {
    background-color: #4a7c59;
    color: #f0ead2;
    border: 2px solid #2e5c3a;
    box-shadow: 2px 2px 0 #1e3d26;
}

.task-remove {
    background-color: #8b3a2f;
    color: #f0ead2;
    border: 2px solid #5c2218;
    box-shadow: 2px 2px 0 #3a1510;
}

.task-complete:hover, .task-remove:hover {
    filter: brightness(1.12);
}

.task-complete:active, .task-remove:active {
    transform: translate(2px, 2px);
    box-shadow: 0 0 0 transparent;
}
</style>