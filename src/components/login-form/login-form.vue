<template>
  <Form
    ref="loginForm"
    :model="form"
    :rules="rules"
    @keydown.enter.native="handleSubmit"
  >
    <FormItem prop="email">
      <Input v-model="form.email" placeholder="请输入用户名">
        <span slot="prepend">
          <Icon :size="16" type="ios-person"></Icon>
        </span>
      </Input>
    </FormItem>
    <FormItem prop="password">
      <Input
        type="password"
        class="lxlPas"
        v-model="form.password"
        password
        placeholder="请输入密码"
      >
        <span slot="prepend">
          <Icon :size="14" type="md-lock"></Icon>
        </span>
      </Input>
    </FormItem>
    <FormItem prop="code">
      <Input
        type="text"
        v-model="form.code"
        style="width: 140px;"
        placeholder="请输入验证码"
      >
        <span slot="prepend">
          <Icon :size="14" type="ios-analytics" />
        </span>
      </Input>
      <span
        class="svg"
        style="color: #c00;"
        @click="_getCode()"
        v-html="svg"
        ></span
      >
    </FormItem>
    <FormItem>
      <Button @click="handleSubmit" type="primary" long>登录</Button>
    </FormItem>
  </Form>
</template>
<script>
import { getCode } from '@/api/login'
import { v4 as uuidv4 } from 'uuid'
export default {
  name: 'LoginForm',
  props: {
    userNameRules: {
      type: Array,
      default: () => {
        return [
          { required: true, message: '账号不能为空', trigger: 'blur' },
          { type: 'email', message: '不是个正确的邮箱', trigger: 'blur' }
        ]
      }
    },
    passwordRules: {
      type: Array,
      default: () => {
        return [
          { required: true, message: '密码不能为空', trigger: 'blur' }
          // {
          //   type: 'string',
          //   min: 8,
          //   message: '密码不能小于8位数',
          //   trigger: 'blur'
          // }
        ]
      }
    },
    codeRules: {
      type: Array,
      default: () => {
        return [{ required: true, message: '验证码不能为空', trigger: 'blur' }]
      }
    }
  },
  data() {
    return {
      form: {
        email: '1847426505@qq.com',
        password: '123456',
        code: '',
        sid: ''
      },
      svg: ''
    }
  },
  mounted() {
    let sid = ''
    if (localStorage.getItem('sid')) {
      sid = localStorage.getItem('sid')
    } else {
      sid = uuidv4()
      localStorage.setItem('sid', sid)
    }
    this.$store.commit('setSid', sid)
    this.form.sid = sid
    this._getCode()
  },
  computed: {
    rules() {
      return {
        email: this.userNameRules,
        password: this.passwordRules,
        code: this.codeRules
      }
    }
  },
  methods: {
    _getCode() {
      const sid = this.$store.state.sid
      getCode(sid).then(res => {
        if (res.code === 200) {
          this.svg = res.data
        }
      })
    },
    handleSubmit() {
      this.$refs.loginForm.validate(valid => {
        if (valid) {
          this.$emit('on-success-valid', { ...this.form })
        }
      })
    }
  }
}
</script>
<style lang="scss">
.lxlPas {
  .ivu-input-suffix {
    z-index: 100;
  }
}
.ivu-form-item-content{
  display: flex;
  .svg {
    flex: 1;
    // width: 100px;
    margin-left: 4px;
    position: relative;
    svg{
      width: 100%;
      position: absolute;
      top: -6px;
    }
  }
}

</style>
