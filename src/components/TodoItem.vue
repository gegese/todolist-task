<template>
  <div class="col-md-3 todo-card m-3 p-0 align-items-baseline" :key="todo.id">
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
          <i class="far fa-trash-alt"></i>
        </div>
      </div>
      <div class="isDone">
          <div class="circle-check-button">
            <i class="fas fa-check"></i>
          </div>
      </div>
    </div>
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
        }
      }
    }
  }
</style>
