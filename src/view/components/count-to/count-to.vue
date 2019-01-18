<template>
 <Row class="card-doctor">
      <Col span="5" v-for="(value,index) in formInline" :key="value.index">
        <Card>
          <div style="text-align:center" >
              <img :src="value.doctor_image" alt="" class="doctor-img">
              <p>医生姓名:{{value.doctor_name}}</p>
              <p>医生所在科室:{{value.o_name}}</p>
              <p>医生工作地点:{{value.doctor_place}}</p>
              <p>医生职称:{{value.doctor_job}}</p>
              <p>挂号金额:{{value.doctor_price}}</p>
              <p>医生简介:{{value.doctor_message}}</p>
              <Button type="primary" @click="correct(index)">点击进行修改</Button>
          </div>
        </Card>
      </Col>
      <div  @click="Add" class="add">
        <Col span="5" class="add">
          <Card>
            <div style="text-align:center">
                <Icon class="md-add" type="md-add" size="200" />
            </div>
          </Card>
        </Col>
      </div>
      <div class="correct">
        <Modal
          v-model="modal1"
          title="修改医生信息"
          @on-ok="ok"
          @on-cancel="cancel"
          cancelText= '删除'
          :closable="false"
          :mask-closable="false"
          >
          <Form>
            <input @change="uploadPhoto($event)" type="file" class="kyc-passin">
            <img :src="correctdoc.doctor_image" alt="" class="reimg">
            </FormItem>
              <Input v-model="correctdoc.doctor_name" class="correctinput" placeholder="医生姓名" name="name">
                <Icon type="ios-contact-outline" slot="prefix" />
              </Input>
            </FormItem>
            <Select v-model="correctdoc.doctor_office" class="correctinput">
              <Option v-for="item in formInline2" :value="item.oid" :key="item.oid">{{ item.o_name }}</Option>
            </Select>
            <Input v-model="correctdoc.doctor_place" placeholder="医生工作地点" class="correctinput" name="goal">
              <Icon type="ios-briefcase-outline" slot="prefix" />
            </Input>
            <Input v-model="correctdoc.doctor_job" placeholder="医生职称" class="correctinput" name="time">
              <Icon type="ios-bookmark-outline" slot="prefix" />
            </Input>
            <Input v-model="correctdoc.doctor_price" placeholder="挂号金额" class="correctinput" name="special">
              <Icon type="ios-medkit-outline" slot="prefix" />
            </Input>
            <Input v-model="correctdoc.doctor_message" type="textarea" :autosize="{minRows: 3,maxRows: 5}" placeholder="医生简介" class="correctinput" name="desc">
              <Icon type="ios-paper-outline" slot="prefix" />
            </Input>
          </Form>
        </Modal>
      </div>
 </Row>
</template>

<script>
import doctor from '@/assets/images/doctor.jpg'
import axios from 'axios'
export default {
  name: 'count_to',
  data () {
    return {
      doctor,
      modal1: false,
      formInline: [
      ],
      formInline2:[
      ],
      correctdoc: {

      }
    }
  },
  created () {
    this.send();
    this.send2()
  },
  methods: {
    Add: function () {
      this.$router.push({
        name: 'drag_list_page'
      })
    },
    ok () {
      this.$Message.info('Clicked ok')
      axios({
        url: 'http://localhost/zyy/doctor/redoc',
        method: 'post',
        data: this.correctdoc,
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
    cancel () {
      this.$Message.info('Clicked cancel')

      axios({
        url: 'http://localhost/zyy/doctor/deldoc',
        method: 'post',
        data: this.correctdoc,
        transformRequest: function (obj) {
          var str = []
          for (var p in obj) {
            str.push(encodeURIComponent(p) + '=' + encodeURIComponent(obj[p]))
          }
          return str.join('&')
        }
      }).then(res => {
        console.log(res)
        this.send();
      }).catch(err => {
        console.log(err)
      })
    },
    correct (index) {
      this.modal1 = true
      this.correctdoc = this.formInline[index]
      console.log(this.correctdoc)
    },
    send () {
      axios({
        method: 'get',
        url: 'http://localhost/zyy/doctor/getdoc'
      }).then((res) => {
        this.formInline = res.data
        // console.log(this.formInline)
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
            this.correctdoc.doctor_image=imgcode
            console.log(this.correctdoc.doctor_image);
        }
    }
  }
}
</script>

<style>
p{
   overflow: hidden;
    -webkit-line-clamp: 2;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-box-orient: vertical;

}
.reimg{
  width: 150px;
  height: 200px;
  position: absolute;
  left: 350px;
}
.doctor-img{
    height: 200px;
    width: 150px;
}
.card-doctor .ivu-card-body{
  height: 430px;
}
.card-doctor .md-add{
  margin-top:100px ;
}
.card-doctor .add{
  cursor: pointer;
}
.card-doctor .formit{
  width: 300px;
}
.card-doctor p{
  text-align: left;
  margin-left: 35px;
  color: #999;
  font-size: 13px;
  margin-top: 2px;
}
.card-doctor{
  position: relative;
}
.card-doctor button{
  position: absolute;
  bottom: 20px;
  transform: translate(-50%,0)
}
.correctinput{
  margin-bottom: 10px;
  width: 280px;
}

</style>
