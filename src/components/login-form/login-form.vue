<template>
  <Form ref="loginForm" :model="form" :rules="rules" @keydown.enter.native="handleSubmit">
    <FormItem prop="userName">
      <Input v-model="form1.userName" placeholder="请输入用户名">
        <span slot="prepend">
          <Icon :size="16" type="ios-person"></Icon>
        </span>
      </Input>
    </FormItem>
    <FormItem prop="password">
      <Input type="password" v-model="form1.password" placeholder="请输入密码">
        <span slot="prepend">
          <Icon :size="14" type="md-lock"></Icon>
        </span>
      </Input>
    </FormItem>
    <FormItem>
      <Button @click="handleSubmit" type="primary" long>登录</Button>
    </FormItem>
  </Form>
</template>
<script>
import axios from 'axios'
export default {
  name: 'LoginForm',
  props: {
    userNameRules: {
      type: Array,
      default: () => {
        return [
          { required: true, message: '账号不能为空', trigger: 'blur' }
        ]
      }
    },
    passwordRules: {
      type: Array,
      default: () => {
        return [
          { required: true, message: '请稍等', trigger: 'blur' }
        ]
      }
    }
  },
  data () {
    return {
      form: {
        userName: 'super_admin',
        password: '' 
      },
      form1:{
        userName:'',
        password:''
      },
      tag:''
    }
  },
  computed: {
    rules () {
      return {
        userName: this.userNameRules,
        password: this.passwordRules
      }
    }
  },
  methods: {
    handleSubmit () {
      axios({
        url: 'http://localhost/zyy/doctor/selo',
        method: 'post',
        data: this.form1,
        transformRequest: function (obj) {
          var str = []
          for (var p in obj) {
            str.push(encodeURIComponent(p) + '=' + encodeURIComponent(obj[p]))
          }
          return str.join('&')
        }
      }).then(res => {
        this.tag=res.data
        console.log(this.tag)
      if(this.tag){
        this.form.userName='super_admin'
        this.form.password='111'
      }
      else{
        alert('账号或密码错误')
      }
      this.$refs.loginForm.validate((valid) => {
        if (valid) {
          this.$emit('on-success-valid', {
            userName: this.form.userName,
            password: this.form.password
          })
        }
      })
      }).catch(err => {
        console.log(err)
      })
    }
  }
}
</script>
