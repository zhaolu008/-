<template>
  <div class="bigBox">
    <div class="centerBox">
      <h2>登录</h2>
      <input type="text" placeholder="请输入应户名" class="inp" /><br />
      <input type="text" placeholder="请输入密码" class="inp" /><br />
      <div>
        <input type="button" value="登录" @click="btn" class="inp2" />
      </div>
    </div>
  </div>
</template>
<script>
export default {
  components: {},
  data() {
    return {};
  },
  methods: {
      btn(){
          this.$router.push('/');
      }
  },
  mounted() {},
};
</script>
<style scoped>
.bigBox {
  width: 100vw;
  height: 100vh;
  background: linear-gradient(to right, #563443, #2f3d60);
}
.centerBox {
  width: 400px;
  height: 200px;
  padding: 20px;
  background: #fff;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
}
input {
  width: 300px;
  height: 30px;
  margin: 10px;
  padding-left: 4px;
  border-radius: 4px;
  outline: none;
  border: none;
  border: 1px solid #c2c6cf;
  /* position: relative; */
}
.inp1::placeholder {
  color: #c2c6cf;
}
.inp2 {
  color: #fff;
  background: cyan;
}
</style>