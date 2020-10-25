<template>
  <div class="container align-left add-box">
    <h2 class="text-uppercase">Add Task</h2>
    <form @submit="submit">
      <div class="form-group text-left">
        <label for="description"><b>Task description</b></label>
        <!-- <input type="text" class="form-control" id="description" aria-describedby="descriptionHelp" v-model="description"> -->
        <textarea class="form-control" id="description" aria-describedby="descriptionHelp" v-model="description" rows="3"></textarea>
        <small id="descriptionHelp" class="form-text text-muted">You have to write explanation of task shortly</small>
      </div>
      <div class="form-group">
        <!-- <label for="isDone">Task is done ?</label> -->
        <input type="hidden" class="form-control" id="isDone" v-model="isDone" value="True">
      </div>
      <button type="submit" class="btn btn-outline-secondary btn-md btn-block " value="Send"><span class="is-normal">Submit</span></button>
    </form>
  </div>
</template>

<script>
import gql from "graphql-tag";
import { InMemoryCache } from "apollo-cache-inmemory";

const ADD_TODO = gql`
  mutation addTodo(
    $description: String!
  ) {
    insert_todos(
      objects: [
        {
          description: $description
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
      const { id, description} = this.$data;
      this.$apollo.mutate({
        mutation: ADD_TODO,
        variables: {
          id,
          description
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
  .is-normal {
    font-weight: 500;
  }
  .add-box {
    border: 1px solid #d2d2d7;
    border-radius: 5px;
    padding: 20px;
  }
</style>
