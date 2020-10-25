<template>
  <div class="col-md-3 todo-card m-3 p-0 align-items-baseline" v-if="todo.isDone === false" :key="todo.id">
    <div class="title-area">
      <span>
        <b>{{ todo.id }} . TASK</b>
      </span>
    </div>
    <div class="description-area">
      <p>
        {{ todo.description }}
      </p>
    </div>
    <div class="operational-area">
      <div class="delete">
        <div class="circle-delete-button">
          <a type="button" @click="deleteTodo(todo)" value="Delete"><i class="far fa-trash-alt red"></i></a>
        </div>
      </div>
      <div class="isDone">
        <a type="button" @click="handleTodoCheck(todo)" value="Done">
          <div class="circle-check-button">
            <i class="fas fa-check green"></i>
          </div>
        </a>
      </div>
    </div>
  </div>
  <div class="col-md-3 todo-card m-3 p-0 align-items-baseline" v-else>
    <div class="title-area">
      <span style="text-decoration: underline line-through overline">
        <b>{{ todo.id }} . TASK</b>
      </span>
    </div>
    <div class="description-area">
      <p style="text-decoration: line-through">
        {{ todo.description }}
      </p>
    </div>
    <div class="operational-area">
      <div class="delete">
        <div class="circle-delete-button">
          <a type="button" @click="deleteTodo(todo)" value="Delete"><i class="far fa-trash-alt red"></i></a>
        </div>
      </div>
      <div class="isDone">
        <a type="button" @click="handleTodoCheck(todo)" value="Done">
          <div class="circle-check-button">
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

export default {
  name: "TodoItem",
  props: ["todo"],
  data() {
    return {
      description: ""
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
    }
  }
};
</script>

<style lang="scss">
  .todo-card {
    display: flex;
    flex-direction: column;
    border: 1px solid #d2d2d7;
    border-radius: 5px;
    .title-area{
      color: #424242;
      font-size: 1.1rem;
      font-weight: 400;
      background-color: #fafafa;
      border-top-left-radius: 5px;
      border-top-right-radius: 5px;
      border-bottom: 1px solid #d2d2d7;
      text-align: left;
      padding: 5px;
      margin-bottom: 10px;
      width: 100%;
      span{
        margin-left: 10px;
      }
    }
    .description-area{
      display: flex;
      flex: 1 0 auto;
      text-align: left;
      color: #000;
      padding: 5px;
      p{
        margin-left: 10px;
      }
    }
    .operational-area{
      display: flex;
      justify-content: space-between;
      flex-shrink: 0;
      background-color: #fafafa;
      border-bottom-left-radius: 5px;
      border-bottom-right-radius: 5px;
      border-top: 1px solid #d2d2d7;
      padding: 5px;
      width: 100%;
      .isDone{
        margin-right: 10px;
        .circle-check-button{
          display: flex;
          align-items: center;
          justify-content: center;
          border: 1px;
          background-color: #fff;
          border: 1px solid #d2d2d7;
          border-radius: 50%;
          width: 32px;
          height: 32px;
          margin-top: 5px;
          margin-bottom: 5px;
          .green {
            color: #5BDB93;
          }
        }
      }
      .delete{
        margin-left: 10px;
        .circle-delete-button{
          display: flex;
          align-items: center;
          justify-content: center;
          border: 1px;
          background-color: #fff;
          border: 1px solid #d2d2d7;
          border-radius: 50%;
          width: 32px;
          height: 32px;
          margin-top: 5px;
          margin-bottom: 5px;
          .red {
            color: #F04022;
          }
        }
      }
    }
  }
</style>
