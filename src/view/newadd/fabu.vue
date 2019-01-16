<template>
  <div class="artical">
    <Form ref="formInline" :model="formInline" :rules="ruleInline" method="post" inline>
      <FormItem prop="title" class="formitem1">
          <Input style="width:540px;" type="text" v-model="formInline.title" placeholder="病例名称" name="name">
              <Icon type="ios-person-outline" slot="prepend"></Icon>
          </Input>
      </FormItem>
      <br>
      <FormItem prop="name" class="formitem1">
          <h3 style="display:inline;">科室：</h3>
        <Select v-model="keshiRoom" clearable style="width:200px">
            <Option v-for="item in keShi" :value="item.value" :key="item.value">{{ item.label }}</Option>
        </Select>
      </FormItem>
      <FormItem prop="type" class="formitem1">
        <h3 style="display:inline;">医生：</h3>
        <Select v-model="model8" clearable style="width:200px">
            <Option v-for="item in doctor" :value="item.value" :key="item.value">{{ item.label }}</Option>
        </Select>
      </FormItem>
      <FormItem>
          
      </FormItem>
    </Form>
        <h3>诊断结果</h3>
        <editor ref="editor" v-model="formInline.desc"/>
        <h3>病例描述</h3>
        <editor ref="editor" v-model="formInline.desc"/>
        <h3>医嘱</h3>
        <editor ref="editor" v-model="formInline.desc"/>
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
      ruleInline: {
        title: [
          { required: true, message: '请填写病例名称', trigger: 'blur' }
        ],
        
      }
    }
  },
  methods: {
    submit () {
      alert('添加成功')
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
</style>
