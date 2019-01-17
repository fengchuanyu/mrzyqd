<template>
    <div class="form">
        <Upload v-model="formInline.img">
            <Button icon="ios-cloud-upload-outline">点击上传照片</Button>
        </Upload>
         <Form ref="formInline" :model="formInline" :rules="ruleInline" method="post" inline>
            <FormItem prop="name" class="formitem">
                <Input type="text" v-model="formInline.name" placeholder="医生姓名" name="name">
                    <Icon type="ios-person-outline" slot="prepend"></Icon>
                </Input>
            </FormItem>
            <FormItem prop="office" class="formitem">
                <Select v-model="formInline.office" class="correctinput">
                    <Option v-for="item in formInline2" :value="item.oid" :key="item.oid">{{ item.o_name }}</Option>
                </Select>
            </FormItem>
            <FormItem prop="place" class="formitem">
                <Input type="text" v-model="formInline.place" placeholder="医生工作地点" name="place">
                    <Icon type="ios-lock-outline" slot="prepend"></Icon>
                </Input>
            </FormItem>
            <FormItem prop="job" class="formitem">
                <Input type="text" v-model="formInline.job" placeholder="医生职称" name="job">
                    <Icon type="ios-bookmark-outline" slot="prepend"></Icon>
                </Input>
            </FormItem>
            <FormItem prop="price" class="formitem">
                <Input type="text" v-model="formInline.price" placeholder="挂号金额" name="price">
                    <Icon type="ios-bookmark-outline" slot="prepend"></Icon>
                </Input>
            </FormItem>
            <FormItem  prop="desc" class="formitem">
                <Input v-model="formInline.desc" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="医生简介" name="desc"></Input>
            </FormItem>
            <FormItem>
                <Button type="primary" @click="submit()">点击保存</Button>
                <!-- <input type="submit" @click="handleSubmit('formInline')" id="submit"> -->
            </FormItem>
        </Form>
    </div>
</template>
<script>
import axios from 'axios'
export default {
  data () {
    return {
      formInline: {
        name: '',
        office: '',
        place: '',
        desc: '',
        job: '',
        price:'',
        img: ''
      },
      formInline2:[
          
      ],
      ruleInline: {
        name: [
          { required: true, message: '请填写医生姓名', trigger: 'blur' }
        ],
        place: [
          { required: true, message: '请填写医生工作地点', trigger: 'blur' }
        ],
        job: [
          { required: true, message: '请填写医生职称', trigger: 'blur' }
        ],
        desc: [
          { required: true, message: '请填写医生简介', trigger: 'blur' }
        ],
        price: [
          { required: true, message: '请填写挂号价格', trigger: 'blur' }
        ]
      }
    }
  },
  created () {
      this.send2()
  },
  methods: {
    handleSubmit (name) {
      this.$refs[name].validate((valid) => {
        if (valid) {
          this.$Message.success('Success!')
        } else {
          this.$Message.error('Fail!')
        }
      })
    },
    submit () {
      alert('添加成功')
      axios({
        url: 'http://localhost/zyy/doctor/adddoc',
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
    },
    send2 () {
      axios({
        method: 'get',
        url: 'http://localhost/zyy/doctor/getoffice'
      }).then((res) => {
        this.formInline2 = res.data.data
        console.log(this.formInline2)
      })
    }
  }
}
</script>
<style>
    .form{
        /* background: red; */
        position: relative;
        left: 450px;
        width: 100px;
    }
    .formitem{
        /* background: blue; */
        position: absolute;
        width: 300px;
    }
    .formitem:nth-child(1){
        top: 0;
    }
    .formitem:nth-child(2){
        top: 70px;
    }
    .formitem:nth-child(3){
        top: 140px;
    }
    .formitem:nth-child(4){
        top: 210px;
    }
    .formitem:nth-child(5){
        top: 280px;
    }
    .formitem:nth-child(6){
        top: 350px;
    }

    .form button:nth-child(1){
        position: absolute;
        top: 400px;
    }
    .form button:nth-child(2){
        position: absolute;
        left: -350px;
        top: 0px;
        /* width: 100px; */
    }
</style>
