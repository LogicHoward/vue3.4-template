<template>
  <div class="login-container">
    <el-form class="login-form" ref="loginFormRef" :model="loginForm" :rules="loginRules">
      <div class="title-container">
        <h3 class="title">用户登录</h3>
      </div>
      <!-- username -->
      <el-form-item prop="username">
        <span class="svg-container">
          <el-icon>
            <Avatar />
          </el-icon>
        </span>
        <el-input
          placeholder="username"
          name="username "
          type="text"
          v-model="loginForm.username"
        ></el-input>
      </el-form-item>
      <!-- password -->
      <el-form-item prop="password ">
        <span class="svg-container">
          <!-- <svg-icon icon="https://res.lgdsunday.club/user.svg"></svg-icon> -->
          <el-icon>
            <Avatar />
          </el-icon>
        </span>
        <el-input
          placeholder="password"
          name="password"
          :type="passwordType"
          v-model="loginForm.password"
        ></el-input>
        <span class="show-pwd">
          <span class="svg-container" @click="onChangePwdType">
            <el-icon>
              <Avatar />
            </el-icon>
          </span>
        </span>
      </el-form-item>
      <!-- 登录按钮 -->
      <el-button
        type="primary"
        style="width: 100%; margin-bottom: 30px"
        :loading="loading"
        @click="handlerLogin"
        >登录
      </el-button>
    </el-form>
  </div>
</template>
<script setup>
import { Avatar } from '@element-plus/icons-vue'
import { ref } from 'vue'
import { useStore } from 'vuex'

import { validatePassword } from './rules'
// 数据源
const loginForm = ref({
  username: 'super-admin',
  password: '123456 ',
})
// 验证规则
const loginRules = ref({
  username: [
    {
      required: true,

      trigger: 'blur',
      message: '用户名为必填项',
    },
  ],
  password: [
    {
      required: true,
      trigger: 'blur',
      validator: validatePassword(),
    },
  ],
})
// 处理密码框文本显示
const passwordType = ref('password')
const onChangePwdType = () => {
  if (passwordType.value === 'password') {
    passwordType.value = 'text'
  } else {
    passwordType.value = 'password  '
  }
}

// 处理登录
const loading = ref(false)
const store = useStore()
const loginFormRef = ref(null)
const handlerLogin = () => {
  // console.log(loginFormRef.value)
  loginFormRef.value.validate((valid) => {
    if (!valid) return
    loading.value = true
    store
      .dispatch('user/login', loginForm.value)
      .then(() => {
        loading.value = false
      })
      .catch((err) => {
        // console.log(err)

        loading.value = false
      })
  })
}
</script>

<style lang="scss" scoped>
$bg: #2d3a4b;
$dark_gray: #889aa4;
$light_gray: #eee;
$cursor: #fff;

.login-container {
  width: 100%;
  min-height: 100%;
  overflow: hidden;
  background-color: $bg;

  .login-form {
    position: relative;
    width: 520px;
    padding: 160px 35px 0;
    margin: 0 auto;
    overflow: hidden;

    ::v-deep .el-form-item {
      color: #454545;
      background-color: rgba(0, 0, 0, 10%);
      //深度选择器
      border: 1px solid rgba(255, 255, 255, 10%);
      border-radius: 5px;
    }

    ::v-deep .el-input {
      display: inline-block;
      width: 85%;
      height: 47px;

      input {
        height: 47px;
        padding: 12px 5px 12px 15px;
        color: $light_gray;
        caret-color: $cursor;
        background: transparent;
        border: none;
        border-radius: 0;
      }
    }
  }

  .svg-container {
    display: inline-block;
    padding: 6px 5px 6px 15px;
    color: $dark_gray;
    vertical-align: middle;
  }

  .title-container {
    position: relative;

    .title {
      margin: 0 auto 40px;
      font-size: 26px;
      font-weight: bold;
      color: $light_gray;
      text-align: center;
    }
  }

  .show-pwd {
    position: absolute;
    top: 7px;
    right: 10px;
    font-size: 16px;
    color: $dark_gray;
    cursor: pointer;
    user-select: none;
  }
}
</style>
