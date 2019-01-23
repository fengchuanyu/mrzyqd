<template>
  <div class="artical">
    <Form ref="formInline" :model="formInline"method="post" inline>
      <FormItem prop="title" class="formitem1">
          <Input style="width:540px;" type="text" v-model="search.title" placeholder="病例名称" name="name">
              <Icon type="ios-person-outline" slot="prepend"></Icon>
          </Input>
      </FormItem>
      <br>
      <FormItem prop="name" class="formitem1">
          <h3 style="display:inline;">科室：</h3>
        <Select v-model="search.oid" clearable style="width:200px" class="select">
            <Option v-for="item in office" :value="item.oid" :key="item.oid">{{ item.o_name }}</Option>
        </Select>
      </FormItem>
      <FormItem prop="type" class="formitem1">
        <h3 style="display:inline;">医生：</h3>
        <Select v-model="search.did" clearable style="width:200px" class="select">
            <Option v-for="item in doc" :value="item.did" :key="item.did">{{ item.doctor_name }}</Option>
        </Select>
      </FormItem>
      <FormItem>
        <h3 style="display:inline;">病人：</h3>
        <Select v-model="search.uid" clearable style="width:200px" class="select" placeholder="请先选择医生科室点击搜索" filterable>
            <Option v-for="item in sea" :value="item.uid" :key="item.uid">{{item.r_date}}  {{ item.user_name }}</Option>
        </Select>
      </FormItem>
      <Button type="primary" @click="sear()">点击搜索病人</Button>
        <h3>病例描述</h3>
        <editor ref="editor" v-model="search.desc"/>
        <h3>诊断结果</h3>
        <editor ref="editor" v-model="search.com"/>
        <h3>医生方案</h3>
        <editor ref="editor" v-model="search.fang"/>
        <h3>注意事项</h3>
        <editor ref="editor" v-model="search.zhuyi"/>
    </Form>
    <Button type="primary" @click="submit()">发布</Button>
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
        desc: ''
      },
      office: {

      },
      doc: {

      },
      search: {
        title:'',
        oid:'',
        did:'',
        uid:'',
        desc:'',
        com:'',
        fang:'',
        date:'',
        zhuyi:''
      },
      sea: {

      },
      ruleInline: {
        title: [
          { required: true, message: '请填写病例名称', trigger: 'blur' }
        ],
        
      }
    }
  },
  created () {
    this.send()
    this.send2()
  },
  methods: {
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
        url: 'http://localhost/zyy/doctor/getoffice'
      }).then((res) => {
        this.office = res.data.data
        console.log(this.office)
      })
    },
    sear () {
      axios({
        url: 'http://localhost/zyy/user/seuser',
        method: 'post',
        data: this.search,
        transformRequest: function (obj) {
          var str = []
          for (var p in obj) {
            str.push(encodeURIComponent(p) + '=' + encodeURIComponent(obj[p]))
          }
          return str.join('&')
        }
      }).then(res => {
        this.sea=res.data.data
        console.log(this.sea)
      }).catch(err => {
        console.log(err)
      })
    },
    submit () {
      this.search.date=this.sea[0].r_date
      // this.search.date=this.sea.r_date
      axios({
        url: 'http://localhost/zyy/user/addcase',
        method: 'post',
        data: this.search,
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
    }
  }
}
</script>
<style>
    .formitem1{
        width: 300px;
    }
    .select{
      position: relative;
      z-index: 100000;
    }
</style>
