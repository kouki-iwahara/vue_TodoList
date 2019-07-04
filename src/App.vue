<template>
  <div id="app">
    <h1>Todo List</h1>
    <form>
      <input type="radio" value="allState" v-model="radioBtnState">全て
      <input type="radio" value="working" v-model="radioBtnState">作業中
      <input type="radio" valie="complete" v-model="radioBtnState">完了
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
        <tr v-for="todo in showTodos" :key="todo.value">
          <td>{{ todo.id }}</td>
          <td>{{ todo.comment }}</td>
          <td><button @click="changeState(todos.indexOf(todo))">{{ todo.stateBtn }}</button></td>
          <td><button @click="cleanTask(todos.indexOf(todo))">{{ todo.delBtn }}</button></td>
        </tr>
      </tbody>
    </table>
    <h2>新規タスクの追加</h2>
    <input type="text" v-model.lazy="comment">
    <button @click="addTask">追加</button>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      comment: '',
      todos: [],
      radioBtnState: 'allState'
    }
  },
  methods:{
    // タスクを追加する関数
    addTask: function () {
      // コメントがあるなら配列todosにタスクをpush
      if(this.comment) {
        this.todos.push({
          comment: this.comment,
          stateBtn: '作業中',
          delBtn: '削除'
        });
        // タスクにidを振る
        this.todos.forEach((todo, index) => {
          todo.id = index + 1;
        });
        this.comment = '';
      }
    },
    // 状態ボタンを切り替える関数
    changeState: function(index) {
      // 状態ボタンが'作業中'なら'完了'、'完了'なら'作業中'に切り替え
      if(this.todos[index].stateBtn === '作業中'){
          this.todos[index].stateBtn = '完了';
      } else {
          this.todos[index].stateBtn = '作業中';
      }
    },
    // タスクを削除する関数
    cleanTask: function(index) {
      this.todos.splice(index, 1);
      // idを振り直す
      this.todos.forEach((todo, index) => {
          todo.id = index + 1;
      });
    }
  },
  computed: {
    // ラジオボタンで画面を切り替える処理
    showTodos: function () {
      // ラジオボタンが'全て'の時、全てのタスクを表示
      if(this.radioBtnState === 'allState') {
        return this.todos;
      } // ラジオボタンが'作業中'の時、作業中のタスクだけを残し表示
        else if(this.radioBtnState === 'working') {
          return this.todos.filter(function(todo){
            return todo.stateBtn === '作業中';
          });
      } // ラジオボタンが'完了'の時、完了のタスクだけを残し表示
        else {
          return this.todos.filter(function(todo){
            return todo.stateBtn === '完了';
          });
      }
    }
  }
}
</script>