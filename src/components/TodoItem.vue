<template>
  <div class="col-md-3 todo-card m-3 p-0 align-items-baseline" v-if="todo.isDone === false" :key="todo.id">
    <div class="title-area">
      <span>
        <b>{{ todo.id }} . TASK</b>
      </span>
    </div>
    <div class="description-area">
      <div class="has-margin" v-if="!editing">
        <p class='text' @click="enableEditing(todo.description)">
          {{todo.description}}
        </p>
      </div>
      <div class="edit-control" v-if="editing">
        <textarea class="form-control mb-2" v-model="tempValue" rows="2"></textarea>
        <button class="btn btn-outline-secondary btn-sm float-right" @click="saveEdit(todo)"> Save </button>
        <button class="btn btn-outline-danger btn-sm mr-2 float-right" @click="disableEditing"> Cancel </button>
      </div>
    </div>
    <div class="operational-area">
      <div class="delete">
        <div class="circle-button">
          <a type="button" @click="deleteTodo(todo)" value="Delete"><i class="far fa-trash-alt red"></i></a>
        </div>
      </div>
      <div class="isDone">
        <a type="button" @click="handleTodoCheck(todo)" value="Done">
          <div class="circle-button">
            <i class="fas fa-check green"></i>
          </div>
        </a>
      </div>
    </div>
  </div>
  <div class="col-md-3 todo-card m-3 p-0 align-items-baseline" v-else>
    <div class="title-area">
      <span class="done">
        <b>{{ todo.id }} . TASK</b>
      </span>
    </div>
    <div class="description-area">
      <p class="done">
        {{ todo.description }}
      </p>
    </div>
    <div class="operational-area">
      <div class="delete">
        <div class="circle-button">
          <a type="button" @click="deleteTodo(todo)" value="Delete"><i class="far fa-trash-alt red"></i></a>
        </div>
      </div>
      <div class="isDone">
        <a type="button" @click="handleTodoCheck(todo)" value="Done">
          <div class="circle-button">
            <i class="fas fa-undo-alt"></i>
          </div>
        </a>
      </div>
    </div>
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

const CHECK_TODO = gql`
   mutation update_todos(
     $id: Int!,
     $isDone: Boolean!
   ) {
     update_todos(
       where: {
         id: {
           _eq: $id
         }
       },
       _set: {
         isDone: $isDone
       }
     ) {
       affected_rows
     }
   }
 `;

 const EDIT_TODO = gql`
    mutation update_todos(
      $id: Int!,
      $description: String!
    ) {
      update_todos(
        where: {
          id: {
            _eq: $id
          }
        },
        _set: {
          description: $description
        }
      ) {
        affected_rows
      }
    }
`;

export default {
  name: "TodoItem",
  props: ["todo"],
  data() {
    return {
      description: "",
      value: "",
      tempValue: null,
      editing: false,
    };
  },
  apollo: {},
  methods: {
    deleteTodo: function(todo) {
      this.$apollo.mutate({
        mutation: DELETE_TODO,
        variables: {
          id: todo.id
        },
        refetchQueries: ["getTodos"]
      });
    },
    handleTodoCheck: function (todo) {
      this.$apollo.mutate({
        mutation: CHECK_TODO,
        variables: {
          id: todo.id,
          isDone: !todo.isDone
        },
        refetchQueries: ["getTodos"],
        update: (store, { data: { update_todos } }) => {
          if (update_todos.affected_rows) {
            console.log(update_todos);
          }
        },
      });
    },
    enableEditing: function(description){
      this.tempValue = description;
      this.editing = true;
    },
    disableEditing: function(){
      this.tempValue = null;
      this.editing = false;
    },
    saveEdit: function(todo) {
      // However we want to save it to the database
      this.value = this.tempValue;
      this.$apollo.mutate({
        mutation: EDIT_TODO,
        variables: {
          id: todo.id,
          description: this.value
        },
        refetchQueries: ["getTodos"],
        update: (store, { data: { update_todos } }) => {
          if (update.todos.affected_rows) {
            console.log(update_todos);
          }
        }
      })
      this.disableEditing();
    }
  }
};
</script>
