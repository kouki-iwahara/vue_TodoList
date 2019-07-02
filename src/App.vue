<template>
  <div id="app">
    <h1>Todo List</h1>
    <form>
      <input type="radio" name="state" checked>全て
      <input type="radio" name="state">作業中
      <input type="radio" name="state">完了
    </form>
    <table>
      <thead>
        <tr>
          <th>ID</th>
          <th>コメント</th>
          <th>状態</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="todo in todos" :key="todo.value">
          <td>{{ todo.id }}</td>
          <td>{{ todo.comment }}</td>
          <td><button @click="changeState(todos.indexOf(todo))">{{ todo.stateBtn }}</button></td>
          <td><button>{{ todo.delBtn }}</button></td>
        </tr>
      </tbody>
    </table>
    <h2>新規タスクの追加</h2>
    <input type="text" v-model.lazy="addComment">
    <button @click="addTodo">追加</button>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      addId: '',
      addComment: '',
      addStateBtn: '作業中',
      addDelBtn: '削除',
      todos: [],
    }
  },
  methods:{
    // タスクを追加する関数
    addTodo: function () {
      if(this.addComment) {
        this.todos.push({
          id: this.addId,
          comment: this.addComment,
          stateBtn: this.addStateBtn,
          delBtn: this.addDelBtn
        });

        this.todos.forEach((todo, index) => {
          todo.id = index + 1;
        });
        this.addComment = '';
      }
    },
    // 状態ボタンを切り替える関数
    changeState: function(index) {
      if(this.todos[index].stateBtn === '作業中'){
          this.todos[index].stateBtn = '完了';
      } else {
          this.todos[index].stateBtn = '作業中';
      }
    }
  }
}
</script>

<style>
  .btn {
    display: none;
  }
</style>


