<template>
  <div class="container-fluid">
    <div class="container">
    <hr>
    <ul class="nav nav-pills nav-justified" id="myTab" role="tablist">
          <li class="nav-item">
            <a class="nav-link active" id="home-tab" data-toggle="tab" href="#home" role="tab" aria-controls="home" aria-selected="true">All</a>
          </li>
      <li class="nav-item">
        <a class="nav-link" id="profile-tab" data-toggle="tab" href="#profile" role="tab" aria-controls="profile" aria-selected="false">Not done</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" id="contact-tab" data-toggle="tab" href="#contact" role="tab" aria-controls="contact" aria-selected="false">Done</a>
      </li>
    </ul>
    <hr>
    </div>
    <div class="tab-content" id="myTabContent">
      <div class="tab-pane fade show active" id="home" role="tabpanel" aria-labelledby="home-tab">
        <div class="row justify-content-center">
          <todo-item v-for="todo in todos" :key="todo.id" :todo="todo"></todo-item>
        </div>
      </div>
      <div class="tab-pane fade" id="profile" role="tabpanel" aria-labelledby="profile-tab">
        <div class="row justify-content-center">
          <todo-item v-for="todo in todos" :key="todo.id" :todo="todo" v-if="todo.isDone === false"></todo-item>
        </div>
      </div>
      <div class="tab-pane fade" id="contact" role="tabpanel" aria-labelledby="contact-tab">
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
