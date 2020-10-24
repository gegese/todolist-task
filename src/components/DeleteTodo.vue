<template>
    <div class="circle-delete-button">
      <a type="button" @click="deleteTodo" value="Delete" :key="todoId"><i class="far fa-trash-alt"></i></a>
    </div>
</template>

<script>
import gql from "graphql-tag";
import { InMemoryCache } from "apollo-cache-inmemory";

const DELETE_TODO = gql`
  mutation deleteTodo(
    $id: Int!
  ) {
    delete_todos(
      where: {
        id: {
          _eq: $id
        }
      }
    ) {
      affected_rows
    }
  }
`;
export default {
  name: "DeleteTodo",
  props: ["todoId"],
  data() {
    return {
      description: "",
      isDone: ""
    };
  },
  apollo: {},
  methods: {
    deleteTodo() {
      const { id } = todoId;
      this.$apollo.mutate({
        mutation: DELETE_TODO,
        variables: {
          id
        },
        refetchQueries: ["getTodos"]
      });
    }
  }
};
</script>
