<template>
  <el-card class="box-card">
    <!-- 如果是原生的input，使用 @keyup.enter就可以，若是使用了element-ui，则要加上native限制符，因为element-ui把input进行了封装 -->
    <el-input v-model.trim="text" placeholder="请输入内容" @keyup.enter.native="addTodo"></el-input>
    <!-- <div v-for="o in 4" :key="o" class="text item">
      {{'列表内容 ' + o }}
    </div>-->
    <div class="todo_list">
      <transition-group name="list">
        <li
          :class="['text','todo', { delete_todo: item.completed }]"
          v-for="(item, index) in todoListComputed"
          :key="index"
          v-on:click="toggleComplete(item)"
        >
          {{item.text}}
          <el-button type="text" icon="el-icon-delete" circle @click="deleteTodo(index)"></el-button>
        </li>
      </transition-group>
    </div>
    <el-button @click="getAll">全部{{' ' + todoList.length}}</el-button>
    <el-button @click="getCompleted">已完成{{' ' + todoListCompleted.length}}</el-button>
    <el-button @click="getnoCompleted">未完成{{' ' + todoList.length - todoListCompleted.length}}</el-button>
  </el-card>
</template>

<script>
import { Promise } from 'q';
import { setTimeout } from 'timers';
/* eslint-disable */
const todoList = [
  {
    id: 1,
    text: "学习vue",
    completed: false
  },
  {
    id: 2,
    text: "今天心情不错",
    completed: false
  }
];
const mockData = [
  {
    id: 4,
    text: '明天 8 点起床跑步',
    completed: false
  },
  {
    id: 8,
    text: '今晚上吃蔬菜',
    completed: false
  }
]
const ALL = "ALL"; //已完成
const COMPLETED = "COMPLETED"; //已完成
const TODO = "TODO"; //待完成

export default {
  data() {
    return {
      text: "",
      todoList,
      completedLen: todoList.filter(todo => todo.completed == true).length,
      noCompletedLen: 0,
      currentStatus: ALL
    };
  },

  async created() {
    this._fetchData().then(res => {
      console.log('res', res);
      this.todoList = this.todoList.concat(res);
    }).catch(() => {
      this.$message.error('获取数据失败');
      
    })
  },

  methods: {
    _fetchData() {
      return new Promise(function(resolve) {
        setTimeout(function() {
          resolve(mockData)
        }, 1500)
      })
    },

    addTodo() {
      const text = this.text;
      const id =
        this.todoList.reduce((maxId, todo) => Math.max(maxId, todo.id), -1) + 1;
      if(!text) { 
        this.$message.warning('请输入内容');
        return;
      }
      this.todoList.push({
        id,
        text,
        completed: false
      });
      this.text = "";
    },
    toggleComplete(item) {
      item.completed = !item.completed;
    },
    deleteTodo(index) {
      this.todoList.splice(index, 1);
    },
    getAll() {
      this.currentStatus = ALL;
    },
    getCompleted() {
      this.currentStatus = COMPLETED;
    },
    getnoCompleted() {
      this.currentStatus = TODO;
    }
  },
  computed: {
    todoListCompleted() {
      return this.todoList.filter(todo => todo.completed !== false);
    },
    todoListComputed() {
      const { currentStatus } = this;
      if (currentStatus === ALL) return this.todoList;
      if (currentStatus === COMPLETED)
        return this.todoList.filter(todo => todo.completed !== false);
      if (currentStatus === TODO)
        return this.todoList.filter(todo => todo.completed == false);
    }
  }
};
</script>

<style>
.text {
  font-size: 16px;
}

.todo {
  padding: 10px 0;
  cursor: pointer;
}

.box-card {
  width: 480px;
}

.delete_todo {
  text-decoration: line-through;
}

.todo_list {
  padding: 15px;
}

.list-enter-active, .list-leave-active {
  transition: all 1s;
}
.list-enter, .list-leave-to
/* .list-leave-active for below version 2.1.8 */ {
  opacity: 0;
  transform: translateY(30px);
}
</style>

