<template>
  <div class="container-fluid">
    <div class="container mt-4 has-border">
    <ul class="nav nav-pills nav-justified" id="situationTab" role="tablist">
          <li class="nav-item">
            <a class="nav-link active" id="all-tab" data-toggle="tab" href="#all" role="tab" aria-controls="all" aria-selected="true">All</a>
          </li>
      <li class="nav-item">
        <a class="nav-link" id="not-done-tab" data-toggle="tab" href="#not-done" role="tab" aria-controls="not-done" aria-selected="false">Not done</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" id="done-tab" data-toggle="tab" href="#done" role="tab" aria-controls="done" aria-selected="false">Done</a>
      </li>
    </ul>
    </div>
    <div class="tab-content" id="situationTabContent">
      <div class="tab-pane fade show active" id="all" role="tabpanel" aria-labelledby="all-tab">
        <div class="row justify-content-center">
          <todo-item v-for="todo in todos" :key="todo.id" :todo="todo"></todo-item>
        </div>
      </div>
      <div class="tab-pane fade" id="not-done" role="tabpanel" aria-labelledby="not-done-tab">
        <div class="row justify-content-center">
          <todo-item v-for="todo in todos" :key="todo.id" :todo="todo" v-if="todo.isDone === false"></todo-item>
        </div>
      </div>
      <div class="tab-pane fade" id="done" role="tabpanel" aria-labelledby="done-tab">
        <div class="row justify-content-center">
          <todo-item v-for="todo in todos" :key="todo.id" :todo="todo" v-if="todo.isDone === true"></todo-item>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import TodoItem from "./TodoItem";
import gql from "graphql-tag";

const GET_TODOS = gql`
  query getTodos {
    todos(order_by: { id: desc }) {
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
