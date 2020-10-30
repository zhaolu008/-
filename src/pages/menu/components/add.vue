<template>
  <div>
    <el-dialog :title="info.title" :visible.sync="info.isShow">
      <el-form :model="form">
        <el-form-item label="菜单名称" label-width="120px">
          <el-input v-model="form.title" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="上级菜单" label-width="120px">
          <el-select
            v-model="form.pid"
            placeholder="请选择活动区域"
            @change="changePid"
          >
            <el-option label="--请选择--" value="" disabled></el-option>
            <el-option label="顶级菜单" :value="0"></el-option>
            <el-option
              v-for="item in list"
              :key="item.id"
              :label="item.title"
              :value="item.id"
            ></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="菜单类型" label-width="120px">
          <el-radio v-model="form.type" :label="1">目录</el-radio>
          <el-radio v-model="form.type" :label="2">菜单</el-radio>
        </el-form-item>
        <el-form-item
          label="菜单图标"
          label-width="120px"
          v-if="form.type == 1"
        >
          <el-select v-model="form.icon" placeholder="请选择图标">
            <el-option label="--请选择--" value disabled></el-option>
            <el-option v-for="item in icons" :key="item" :value="item">
              <i :class="item"></i>
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item
          label="菜单地址"
          label-width="120px"
          v-if="form.type == 2"
        >
          <el-select v-model="form.url" placeholder="请选择地址">
            <el-option label="--请选择--" value disabled></el-option>
            <el-option
              v-for="item in indexRouters"
              :key="item.path"
              :value="'/' + item.path"
              :label="item.name"
            ></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="状态" label-width="120px">
          <el-switch
            v-model="form.status"
            :active-value="1"
            :inactive-value="2"
          ></el-switch>
          <!-- {{ form.status }} -->
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="cancle">取 消</el-button>
        <el-button
          v-if="info.title == '编辑菜单'"
          type="primary"
          @click="upData"
          >编辑</el-button
        >
        <el-button type="primary" @click="add" v-else>添加</el-button>
      </div>
    </el-dialog>
  </div>
</template>
<script>
import { indexRouters } from "../../../router/index.js";
import axios from "axios";
import qs from "qs";
import { successAlert, warningAlert } from "../../../utils/alert";
export default {
  props: ["info", "list"],
  components: {},
  data() {
    return {
      isShow: true,
      form: {
        pid: "",
        title: "",
        icon: "",
        type: "",
        url: "",
        status: 1,
      },
      //图标数据
      icons: [
        "el-icon-camera-solid",
        "el-icon-video-camera-solid",
        "el-icon-s-flag",
        "el-icon-share",
      ],
      /* formLabelWidth: "120px", */
      indexRouters,
    };
  },
  methods: {
    //取消
    cancle() {
      this.info.isShow = false;
    },
    //重置form数据
    resForm() {
      this.form = {
        pid: "",
        title: "",
        icon: "",
        type: "",
        url: "",
        status: 1,
      };
    },
    //顶级菜单变化目录的对应
    changePid() {
      if (this.form.pid == 0) {
        this.form.type = 1;
      } else {
        this.form.type = 2;
      }
    },
    //添加
    add() {
      // console.log(this.form)
      //发请求 向数据库传参
      axios({
        url: "/api/api/menuadd",
        method: "post",
        data: qs.stringify(this.form),
      }).then((res) => {
        if (res.data.code == 200) {
          //1.弹框消失
          this.cancle();
          //2.弹个成功
          successAlert(res.data.msg);
          //3.重新form
          this.resForm();
          //4.刷新list数据
          this.$emit("init");
          console.log(res);
        } else {
          warningAlert(res.data.msg);
        }
      });
    },
    //获取某一条的数据
    getOne(id) {
      //发起获取一条数据的请求
      axios({
        url: "/api/api/menuinfo",
        method: "get",
        params: {
          id: id,
        },
      }).then((res) => {
        // console.log(res)
        this.form = res.data.list;
        this.form.id = id;
      });
    },
    //点击编辑按钮
    upData() {
      axios({
        url: "/api/api/menuedit",
        method: "post",
        data: qs.stringify(this.form),
      }).then((res) => {
        if (res.data.code == 200) {
          //1.弹框消失
          this.cancle();
          //2.弹个成功
          successAlert(res.data.msg);
          //3.重新form
          this.resForm();
          //4.刷新list数据
          this.$emit("init");
        } else {
          warningAlert(res.data.msg);
        }
      });
    },
  },
  mounted() {},
};
</script>
<style scoped>
</style>