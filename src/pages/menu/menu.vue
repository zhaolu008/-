<template>
  <div>
    <el-button type="primary" @click="add">添加</el-button>
    <v-add ref="add" :info="info" :list="list" @init="getInit"></v-add>
    <v-list :list="list" @edit="willAdit($event)"></v-list>
  </div>
</template>
<script>
import vAdd from "./components/add";
import vList from "./components/list";
import axios from "axios";
export default {
  components: {
    vAdd,
    vList,
  },
  data() {
    return {
      //父传子
      info: {
        isShow: false,
        title: "添加菜单",
      },
      list: [],
    };
  },
  methods: {
    //点击添加
    add() {
      this.info.isShow = true;
      this.info.title = "添加菜单";
    },
    //获取列表数据
    getInit() {
      axios({
        url: "/api/api/menulist",
        method: "get",
        params: {
          istree: true,
        },
      }).then((res) => {
        this.list = res.data.list;
        // console.log(res);
      });
    },
    //点击编辑显示添加窗口
    willAdit(id) {
      this.info.isShow = true;
      this.info.title = "编辑菜单";
      //根据敌发起获取一条数据的请求
      //父组件调用子组件的方法
      this.$refs.add.getOne(id);
      //重置grom数据
      this.$refs.add.resForm();
    },
  },

  mounted() {
    //一进来就获取list数据
    this.getInit();
  },
};
</script>
<style scoped>
</style>