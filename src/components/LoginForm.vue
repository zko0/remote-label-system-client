<template>
  <el-form
    ref="loginForm"
    :model="loginUser"
    :rules="rules"
    label-width="100px"
    class="loginForm sign-in-form"
  >
    <el-form-item label="邮箱" prop="email">
      <el-input
        v-model="loginUser.email"
        placeholder="Enter Email..."
      ></el-input>
    </el-form-item>
    <el-form-item label="密码" prop="password">
      <el-input
        v-model="loginUser.password"
        type="password"
        placeholder="Enter Password..."
      ></el-input>
    </el-form-item>

    <el-form-item>
      <el-button
        @click="handleLogin('loginForm')"
        type="primary"
        class="submit-btn"
        >登陆</el-button
      >
    </el-form-item>

    <!-- 找回密码 -->
    <div class="tiparea">
      <p>忘记密码？ <a>立即找回</a></p>
    </div>
  </el-form>
</template>

<script lang="ts">
import { ref, getCurrentInstance } from "vue";
import {useRouter} from 'vue-router'
export default {
  props: {
    loginUser: {
      type: Object,
      required: true,
    },
    rules: {
      type: Object,
      required: true,
    },
  },
  setup(props:any) {
    // @ts-ignore
    const { proxy } = getCurrentInstance();
    const router =useRouter();
    // 触发登录方法
    const handleLogin = (formName: string) => {
      proxy.$refs[formName].validate((valid: boolean) => {
        if (valid) {
          proxy.$axios
            .post(
              "http://127.0.0.1:9000/user/doLogin",
              props.loginUser
            )
            .then((res: any) => {
              proxy.$message({
                message: "登录成功",
                type: "success",
              });
            });
            //路由跳转
            router.push("/");
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    };

    return { handleLogin };
  },
};
</script>
<style scoped>
/* form */
.loginForm {
  margin-top: 20px;
  background-color: #fff;
  padding: 20px 40px 20px 20px;
  border-radius: 5px;
  box-shadow: 0px 5px 10px #cccc;
}

.submit-btn {
  width: 100%;
}
.tiparea {
  text-align: right;
  font-size: 12px;
  color: #333;
}
.tiparea p a {
  color: #409eff;
}
</style>
