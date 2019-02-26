<template>
    <div class="form">
        <input @change="uploadPhoto($event)" type="file" class="kyc-passin">
        <img :src="formInline.img" alt="">
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
    },
    uploadPhoto (e) {
        // 利用fileReader对象获取file
        var file = e.target.files[0];
        var filesize = file.size;
        var filename = file.name;
        // 2,621,440   2M
        if (filesize > 2101440) {
            // 图片大于2MB
    
        }
        var reader = new FileReader();
        reader.readAsDataURL(file);
        reader.onload = (e) => {
    
            // 读取到的图片base64 数据编码 将此编码字符串传给后台即可
            var imgcode = e.target.result;
            this.formInline.img=imgcode
            console.log(this.formInline.img);
        }
    }
  }
}
</script>
<style>
    .form img{
        position: absolute;
        width: 200px;
        height: 300px;
        left: -300px;
        top: 50px;
    }
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
        position: relative;
        top: 500px;
    }
    .form button:nth-child(2){
        position: absolute;
        left: -350px;
        top: 0px;
        /* width: 100px; */
    }
    .kyc-passin{
        position: absolute;
        left: -300px;
    }
</style>
