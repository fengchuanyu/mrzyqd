<style lang="less">
  @import './login.less';
</style>

<template>
  <div class="login">
    <div class="login-con">
      <Card icon="log-in" title="欢迎登录" :bordered="false">
        <div class="form-con">
          <login-form @on-success-valid="handleSubmit"></login-form>
          <p class="cpass" @click="repass">点击修改密码</p>
        </div>
      </Card>
    </div>
    <Card 
      v-show="modal1"
      title="修改密码" class="recard">
      <h3>请输入用户名：</h3>
      <i-input v-model="form.userName" placeholder="" name="title"></i-input>
      <h3>请输入旧密码：</h3>
      <i-input v-model="form.password" placeholder="" name="title"></i-input>
      <h3>请输入新密码：</h3>
      <i-input v-model="form.newpass" placeholder="" name="title"></i-input>
      <Button type="primary" @click="rep">保存更改</Button>
    </Card>
  </div>
</template>

<script>
import LoginForm from '_c/login-form'
import { mapActions } from 'vuex'
import axios from 'axios'
export default {
  components: {
    LoginForm
  },
  data () {
    return {
      modal1:false,
      form:{
        userName:'',
        password:'',
        newpass:''
      }
    }
  },
  methods: {
    ...mapActions([
      'handleLogin',
      'getUserInfo'
    ]),
    handleSubmit ({ userName, password }) {
      this.handleLogin({ userName, password }).then(res => {
        this.getUserInfo().then(res => {
          this.$router.push({
            name: 'home'
          })
        })
      })
    },
    repass () {
      this.modal1=true;
    },
    rep () {
      console.log(this.form)
      axios({
        url: 'http://localhost/zyy/doctor/repass',
        method: 'post',
        data: this.form,
        transformRequest: function (obj) {
          var str = []
          for (var p in obj) {
            str.push(encodeURIComponent(p) + '=' + encodeURIComponent(obj[p]))
          }
          return str.join('&')
        }
      }).then(res => {
        console.log(res.data)
        if(res.data==1){
          this.modal1=false;
          alert('修改成功')
        }else{
          alert('用户名或密码错误')
        }
      }).catch(err => {
        console.log(err)
      })
    }
  }
}
</script>

<style>
  .cpass{
    position: relative;
    margin: 0 auto;
    font-size: 13px;
    color:#999;
    /* display: block; */
    text-align: center;
    cursor: pointer;
  }
  .recard{
    width: 500px;
    position: relative;
    margin: 0 auto;
    top: 200px;
  }
</style>
