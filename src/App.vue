<template>
  <div id="container-box">
    <TaskList />
  </div>
</template>

<script>
import TaskList from './components/TaskList.vue';

export default {
  name: 'App',
  components: {
    TaskList
  } 
};
</script>
<style>

*, *::before, *::after {
  box-sizing: border-box;
}

body {
  margin: 0;
  padding: 0;
}

#container-box {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  min-height: 100vh;
  padding: 40px 20px;
  background-color: #2c1f0e;
  background-image:
    repeating-linear-gradient(
      0deg,
      transparent,
      transparent 28px,
      rgba(0,0,0,0.12) 28px,
      rgba(0,0,0,0.12) 29px
    ),
    repeating-linear-gradient(
      90deg,
      transparent,
      transparent 28px,
      rgba(0,0,0,0.05) 28px,
      rgba(0,0,0,0.05) 29px
    );
}
</style>
