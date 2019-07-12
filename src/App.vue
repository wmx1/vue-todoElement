<template>
  <el-card class="box-card">
    <el-input v-model="text" placeholder="请输入内容" @></el-input>
    <!-- <div v-for="o in 4" :key="o" class="text item">
      {{'列表内容 ' + o }}
    </div>-->
    <div class="todo_list">
      <li class="text todo" v-for="(item, index) in todoList" :key="index">
        {{item.text}}
        <el-button type="text" icon="el-icon-delete" circle></el-button>
      </li>
    </div>
    <el-button>全部</el-button> 
    <el-button>已完成</el-button>
    <el-button>未完成</el-button>
  </el-card>
</template>

<script>
const todoList = [{
  id: 1,
  text: '学习vue',
  completed: true
}, {
  id: 2,
  text: '今天心情不错',
  completed: false
}];
export default {
  data() {
    return {
      text: '',
      todoList
    };
  }
};
</script>

<style>
.text {
  font-size: 14px;
}

.todo {
  padding: 10px 0;
}

.box-card {
  width: 480px;
}
</style>
