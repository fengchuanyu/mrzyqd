<template>
  <div class="artical">
    <Form ref="formInline" :model="formInline" :rules="ruleInline" method="post" inline>
      <FormItem prop="title" class="formitem1">
          <Input type="text" v-model="formInline.title" placeholder="文章标题" name="name">
              <Icon type="ios-person-outline" slot="prepend"></Icon>
          </Input>
      </FormItem>
      <FormItem prop="name" class="formitem1">
        <div class="iview-item">
          <Select v-model="formInline.name" placeholder="作者名字">
              <Option v-for="item in doc" :value="item.did" :key="item.did">{{ item.doctor_name }}</Option>
          </Select>
        </div>
      </FormItem>
      <FormItem prop="type" class="formitem1">
        <div class="iview-item">
          <Select v-model="formInline.type" placeholder="文章类型">
              <Option v-for="item in ill" :value="item.cate_id" :key="item.cate_id">{{ item.cate_name }}</Option>
          </Select>
        </div>
      </FormItem>
      <FormItem>
          <Button type="primary" @click="submit()">点击保存</Button>
      </FormItem>
    </Form>
    <editor ref="editor" v-model="formInline.desc"/>
  </div>
</template>

<script>
import axios from 'axios'
import Editor from '_c/editor'
export default {
  name: 'page-list',
  components: {
    Editor
  },
  data () {
    return {
      formInline: {
        title: '',
        name: '',
        type: '',
        desc: '',
        time: ''
      },
      ruleInline: {
        title: [
          { required: true, message: '请填写文章标题', trigger: 'blur' }
        ],
        name: [
          { required: true, message: '请填写作者', trigger: 'blur' }
        ],
        type: [
          { required: true, message: '请填写文章类型', trigger: 'blur' }
        ],
        desc: [
          { required: true, message: '请填写文章内容', trigger: 'blur' }
        ]
      },
      doc: [
      ],
      ill:[]
    }
  },
  created () {
    this.send()
    this.send2()
  },
  methods: {
    submit () {
      var date = new Date();
      var year = date.getFullYear();
      var month = date.getMonth() + 1;
      var day = date.getDate();
      if (month < 10) {
          month = "0" + month;
      }
      if (day < 10) {
          day = "0" + day;
      }
      var nowDate = year + "-" + month + "-" + day;
      this.formInline.time=nowDate
      console.log(this.formInline)
      axios({
        url: 'http://localhost/zyy/doctor/addarticle',
        method: 'post',
        data: this.formInline,
        transformRequest: function (obj) {
          var str = []
          for (var p in obj) {
            str.push(encodeURIComponent(p) + '=' + encodeURIComponent(obj[p]))
          }
          return str.join('&')
        }
      }).then(res => {
        alert('添加成功')
        console.log(res)
      }).catch(err => {
        console.log(err)
      })
    },
    send () {
      axios({
        method: 'get',
        url: 'http://localhost/zyy/doctor/getdoc'
      }).then((res) => {
        this.doc = res.data
        console.log(this.doc)
      })
    },
    send2 () {
      axios({
        method: 'get',
        url: 'http://localhost/zyy/User/allcate'
      }).then((res) => {
        this.ill = res.data;
        console.log(this.ill)
      })
    }
  }
}
</script>
<style>
    .formitem1{
        width: 300px;
    }
    select{
        width: 300px;
    }
    .iview-item{
      position: relative;
      z-index: 1000000;
    }
</style>
