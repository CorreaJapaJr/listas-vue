<template>
  <div class="button-group">
    <button @click="$emit('count-change', 'increment', 1)">+1</button>
    <button @click="$emit('count-change', 'decrement', 1)">-1</button>
    <button @click="$emit('count-change', 'reset')" class="reset-btn">Reset</button>
  </div>
</template>

<script>
export default {
  name: "CounterButton",
  // Se quiser usar props para o texto dos botões futuramente:
  props: {
    title: {
      type: String,
      default: 'Botão'
    }
  }
};
</script>

<style scoped>
  .button-group {
    display: flex;
    gap: 10px;
    justify-content: center;
  }

  button {
    background-color: #42b983;
    border: none;
    padding: 10px 20px;
    color: white;
    border-radius: 5px;
    cursor: pointer;
    font-weight: bold;
  }

  button:hover {
    background-color: #369870;
  }

  .reset-btn {
    background-color: #eb4d4b;
  }
  
  .reset-btn:hover {
    background-color: #cf3a3a;
  }
</style>