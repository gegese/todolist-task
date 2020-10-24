<template>
  <div class="container">
    <h4>Add Task</h4>
    <form @submit="submit">
      <div class="form-group">
        <label for="description">Task description</label>
        <input type="text" class="form-control" id="description" aria-describedby="descriptionHelp" v-model="description">
        <small id="descriptionHelp" class="form-text text-muted">You have to write explanation of task shortly</small>
      </div>
      <div class="form-group">
        <label for="isDone">Task is done ?</label>
        <input type="text" class="form-control" id="isDone" v-model="isDone">
      </div>
      <button type="submit" class="btn btn-primary" value="Send">Submit</button>
    </form>
  </div>
</template>

<script>
import gql from "graphql-tag";
import { InMemoryCache } from "apollo-cache-inmemory";

const ADD_TODO = gql`
  mutation addTodo(
    $description: String!
    $isDone: Boolean!
  ) {
    insert_todos(
      objects: [
        {
          description: $description
          isDone: $isDone
        }
      ]
    ) {
      returning {
        id
      }
    }
  }
`;
export default {
  name: "AddTodo",
  data() {
    return {
      description: "",
      isDone: ""
    };
  },
  apollo: {},
  methods: {
    submit(e) {
      e.preventDefault();
      const { id, description, isDone } = this.$data;
      this.$apollo.mutate({
        mutation: ADD_TODO,
        variables: {
          id,
          description,
          isDone
        },
        refetchQueries: ["getTodos"]
      });
    }
  }
};
</script>

<style lang="scss" scoped>
  form{
    margin-bottom: 0;
  }
</style>
