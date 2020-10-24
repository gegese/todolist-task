<template>
  <div class="container-fluid">
    <div class="row justify-content-center">
      <todo-item v-for="todo in todos" :key="todo.id" :todo="todo"></todo-item>
    </div>
  </div>
</template>

<script>
import TodoItem from "./TodoItem";
import gql from "graphql-tag";

const GET_TODOS = gql`
  query getTodos {
    todos {
      id
      description
      isDone
    }
  }
`;

export default {
  name: "TodoList",
  components: { TodoItem },
  data() {
    return {
      todos: []
    };
  },
  apollo: {
    todos: {
      query: GET_TODOS
    }
  }
};
</script>
