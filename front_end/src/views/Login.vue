<template>
  <div id="background">
    <div id="login">
      <div id="account" class="input_div">
        <input class="inputs" v-model="account" @keydown.enter="login" placeholder="Please input"/>
      </div>
      <div id="password" class="input_div">
        <input class="inputs" v-model="password" @keydown.enter="login" placeholder="Please input" />
      </div>
      <div class="input_div" style="padding-top: 40px" @click="login">
        <button id="button">
          登录
        </button>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import { ElMessage } from "element-plus";
import { url } from '@/settings';
import axios from 'axios';
import router from '../router/index'
import {store} from '@/store/store'
import {setCookies} from '@/store/cookie'

const account = ref('user1')
const password = ref('password1')



const login = () => {
  if (account.value == '' || password.value == '') {
    msg_error("请输入账号和密码")
  } else {
    const data = new FormData()
    data.append('account', account.value)
    data.append('password', password.value)
    axios.post(url + '/login', data).then(function (response) {
      const info=response.data;
      if (info['code'] == 0) {
        store.fullname=info['fullname']
        store.account=info['account']
        store.username=info['username']
        store.phone=info['phone']
        setCookies()
        router.push('/main')
      } else if (response.data['code'] == -1) {
        msg_error("账号不存在！")
      } else if (response.data['code'] == 1) {
        msg_error("密码错误！")
      }
    })
  }
}
const msg_error = (msg) => {
  ElMessage.error(msg)
}
</script>

<style scoped>
#background {
  padding: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgb(151, 157, 167);
}

#login {
  width: 75vw;
  height: 35vh;
  min-height: 300px;
  border-radius: 15px;
  background-color: rgb(39, 42, 55);
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  padding-top: 80px;
}

.inputs {
  width: 60%;
  height: 50px;
  background-color: rgb(66, 70, 86);
  border-radius: 10px;
  border: 2px solid rgb(34, 135, 225);
  padding: 10px;
  box-sizing: border-box;
  transition: 0.2s;
  font-size: 20px;
  color: #fff;
  font-weight: 100;
  outline: none;
}

.input_div {
  margin-bottom: 20px;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

#account::before {
  content: "账号：";
  font-size: 25px;
  color: #f2f2f2;
}

#password::before {
  content: "密码：";
  font-size: 25px;
  color: #f2f2f2;
}

#button {
  width: 100px;
  height: 50px;
  color: #f2f2f2;
  font-size: 20px;
  background: rgb(34, 135, 225);
  border: 0;
  border-radius: 20px;
}

#button:hover {
  cursor: pointer;
}
</style>
