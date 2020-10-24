<template>
  <div class="col-md-3 todo-card m-3 p-0 align-items-baseline" :key="todo.id">
    <div class="title-area">
      <span> {{ todo.id }} . TASK </span>
    </div>
    <div class="description-area">
      <p>{{ todo.description }}</p>
    </div>
    <span>Situation: {{ todo.isDone}}</span>
  </div>
</template>

<script>
export default {
  name: "TodoItem",
  props: ["todo"]
};
</script>

<style lang="scss">
  .todo-card {
    border: 1px solid #42424250;
    border-radius: 5px;
    .title-area{
      color: #424242;
      font-size: 1.1rem;
      font-weight: 400;
      background-color: #cccccc50;
      border-top-left-radius: 5px;
      border-top-right-radius: 5px;
      border-bottom: 1px solid #ddd;
      text-align: left;
      padding: 5px;
      margin-bottom: 10px;
      span{
        margin-left: 10px;
      }
    }
  }
</style>
