<template>
  <div class="container register">
    <section class="title-section">
      <h1>zchat</h1>
    </section>
    <section class="input-section">
      <van-cell-group
        :model="registerForm"
      >
        <van-field
          ref="username"
          v-model="registerForm.username"
          placeholder="用户名"
          left-icon="manager"
          type="text"
        />
        <van-field
          ref="password"
          v-model="registerForm.password"
          placeholder="密码"
          left-icon="lock"
          type="password"
        />
        <van-field
          ref="password"
          v-model="registerForm.passwordAgain"
          placeholder="再次输入密码"
          left-icon="lock"
          type="password"
        />
      </van-cell-group>
    </section>
    <section class="btn-section">
      <van-button
        :loading="loading"
        type="info"
        loading-text="注册中..."
        @click="register"
      >
        注册
      </van-button>
    </section>
    <section class="router-section">
      <router-link to="message">
        回到首页
      </router-link>
      <span>|</span>
      <router-link to="login">
        登录用户
      </router-link>
    </section>
  </div>
</template>

<script>
import { Field, Row, Col, CellGroup, Button, Notify } from 'vant'
import { register } from '@/api/user'
export default {
	name: 'Register',
	components: {
		[Field.name]: Field,
		[Row.name]: Row,
    [Col.name]: Col,
    [CellGroup.name]: CellGroup,
    [Button.name]: Button
	},
	data() {
		return {
      registerForm: {
        username: '',
        password: '',
        passwordAagin: ''
      },
      loading: false
		}
  },
  methods: {
    register() {
      this.loading = true
      register(this.registerForm).then(() => {
        this.loading = false
        Notify({
          message: '注册成功',
          background: '#07c160',
          duration: 3 * 1000
        })
        this.$router.push('login')
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.register {
  padding: 40% 30px 0 30px;
  min-height: 100vh;
  box-sizing: border-box;
  background-color: #f5f5f5;
  .title-section {
    text-align: center;
    h1 {
      margin: 0;
    }
  }
  .input-section {
    margin: 20px 0;
  }
  .btn-section {
    .van-button {
      border-radius: 0px;
      width: 100%;
      height: 40px;
      line-height: 40px;
    }
  }
  .router-section {
    text-align: center;
    margin-top: 150px;
    span {
      margin: 0 5px;
    }
    a, span {
      color: #1989fa;
    }
    a:hover {
      color: #167ce2;
    }
  }
}
</style>
