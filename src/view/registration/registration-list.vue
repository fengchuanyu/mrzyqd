<template>
  <div>
    <Tabs type="card">
        <Tab-pane label="当前挂号">
          <Table border :columns="columns7" :data="data6"></Table>
        </Tab-pane>
        <Tab-pane label="往期挂号">
          <Table border :columns="columns8" :data="data7"></Table>
        </Tab-pane>
    </Tabs>
  </div>
</template>
<script>
import axios from 'axios'
import qqFans from '@/assets/images/qq-fance.jpg'
// import { getData } from '@/api/data'
export default {
  name: 'list_page',
  data () {
    return {
      Data:{

      },
      reg:{

      },
      rreg:[

      ],
      columns7: [
        {
          title: '用户名',
          key: 'user_name',
          render: (h, params) => {
            return h('div', [
              h('Icon', {
                props: {
                  type: 'person'
                }
              }),
              h('strong',{
                style: {
                  cursor: 'pointer'
                },
                on: {
                click: () => {
                  this.user(params.index)
                }
              }
              }, params.row.user_name)
            ]);
          }
        },
        {
          title: '患者姓名',
          key: 'r_name'
        },
        {
          title: '挂号医生',
          key: 'doctor_name',
          render: (h, params) => {
            return h('div', [
              h('strong',{
                style: {
                  cursor: 'pointer'
                },
                on: {
                click: () => {
                  this.doctor(params.index)
                }
              }
              }, params.row.doctor_name)
            ]);
          }
        },
        {
          title: '挂号日期',
          key: 'r_date'
        },
        {
          title: 'Action',
          key: 'action',
          width: 150,
          align: 'center',
          render: (h, params) => {
          return h('div', [
            h('Button', {
              props: {
                type: 'primary',
                size: 'large'
              },
              style: {
                marginRight: '5px'
              },
              on: {
                click: () => {
                  this.show(params.index)
                }
              }
            }, '详情'),
          ]);
          }
        }
      ],
      columns8: [
        {
          title: '用户名',
          key: 'user_name',
          render: (h, params) => {
            return h('div', [
              h('Icon', {
                props: {
                  type: 'person'
                }
              }),
              h('strong',{
                style: {
                  cursor: 'pointer'
                },
                on: {
                click: () => {
                  this.user1(params.index)
                }
              }
              }, params.row.user_name)
            ]);
          }
        },
        {
          title: '患者姓名',
          key: 'r_name'
        },
        {
          title: '挂号医生',
          key: 'doctor_name',
          render: (h, params) => {
            return h('div', [
              h('strong',{
                style: {
                  cursor: 'pointer'
                },
                on: {
                click: () => {
                  this.doctor(params.index)
                }
              }
              }, params.row.doctor_name)
            ]);
          }
        },
        {
          title: '挂号日期',
          key: 'r_date'
        },
        {
          title: 'Action',
          key: 'action',
          width: 150,
          align: 'center',
          render: (h, params) => {
          return h('div', [
            h('Button', {
              props: {
                type: 'primary',
                size: 'large'
              },
              style: {
                marginRight: '5px'
              },
              on: {
                click: () => {
                  this.show1(params.index)
                }
              }
            }, '详情'),
          ]);
        }
      }
    ],
    data6: [

    ],
    data7: [

    ]
    } 
  },
  methods: {
    send () {
      axios({
        method: 'get',
        url: 'http://localhost/zyy/user/allreg'
      }).then((res) => {
        this.data6 = res.data.data
        // console.log(this.data6)
      })
    },
    send2 () {
      axios({
        method: 'get',
        url: 'http://localhost/zyy/user/allreg2'
      }).then((res) => {
        this.data7 = res.data.data
        // console.log(this.data7)
      })
    },
    show (index) {
      this.$Modal.info({
        title: '挂号详情',
        content: `用户名：${this.data6[index].user_name}<br>患者姓名：${this.data6[index].r_name}<br>挂号医生：${this.data6[index].doctor_name}<br>挂号日期：${this.data6[index].r_time}`
      })
    },
    show1 (index) {
      this.$Modal.info({
        title: '挂号详情',
        content: `用户名：${this.data7[index].user_name}<br>患者姓名：${this.data7[index].r_name}<br>挂号医生：${this.data7[index].doctor_name}<br>挂号日期：${this.data6[index].r_time}`
      })
    },
    user (index) {
      this.$Modal.info({
        title: '用户详情',
        content: `用户名：${this.data6[index].user_name}<br>用户性别：${this.data6[index].user_sex}<br>用户电话：${this.data6[index].user_phone}<br>用户身份证号：${this.data6[index].user_idnumber}`
      })
    },
    user1 (index) {
      this.$Modal.info({
        title: '用户详情',
        content: `用户名：${this.data7[index].user_name}<br>用户性别：${this.data7[index].user_sex}<br>用户电话：${this.data7[index].user_phone}<br>用户身份证号：${this.data7[index].user_idnumber}`
      })
    },
    doctor (index) {
      this.$Modal.info({
        title: '医生详情',
        content: `医生姓名：${this.data6[index].doctor_name}<br>医生职位：${this.data6[index].doctor_job}<br>医生工作地点：${this.data6[index].doctor_place}<br>医生擅长：${this.data6[index].doctor_special}`
      })
    },
    doctor1 (index) {
      this.$Modal.info({
        title: '医生详情',
        content: `医生姓名：${this.data7[index].doctor_name}<br>医生职位：${this.data7[index].doctor_job}<br>医生工作地点：${this.data7[index].doctor_place}<br>医生擅长：${this.data7[index].doctor_special}`
      })
    },
    send3 () {
      axios({
        method: 'get',
        url: 'http://localhost/zyy/user/changereg'
      }).then((res) => {
        this.reg = res.data
        this.rreg=this.reg;
        for(var i=0;i<this.rreg.length;i++){
          // var time= new Date().getTime()
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
          // console.log(nowDate)
          var rtime=this.rreg[i].r_date
          // var time2='2019-01-23'
          if(nowDate>=rtime){
            this.rreg[i].r_tag=2
            // console.log(typeof(time),typeof(rtime))
            console.log(0)
          }
          if(rtime>nowDate){
            this.rreg[i].r_tag=1
            console.log(1)
          }
          if(rtime==null){
            this.rreg[i].r_tag=0;
          }
          // console.log(this.rreg[i].r_tag)
        }
        console.log(this.rreg)
        this.info()
      })
    },
    info () {
      for(var i=0;i<this.rreg.length;i++){
        axios({
        url: 'http://localhost/zyy/user/rereg',
        method: 'post',
        data: this.rreg[i],
        transformRequest: function (obj) {
          var str = []
          for (var p in obj) {
            str.push(encodeURIComponent(p) + '=' + encodeURIComponent(obj[p]))
          }
          return str.join('&')
        }
      }).then(res => {
        // console.log(res)
      }).catch(err => {
        console.log(err)
      })
      }
      this.send()
      this.send2()
    }
    /* info () {
        axios({
        url: 'http://localhost/zyy/user/rereg',
        method: 'post',
        data: JSON.stringify(this.rreg),
      }).then(res => {
        console.log(res)
      }).catch(err => {
        console.log(err)
      })
      this.send()
      this.send2()
    } */

  },
  created() {
    this.send3()
  }
}
</script>

<style>
card{
  height: 50px;
  line-height: 50px;
  margin-bottom: 30px;
}
.span{
  margin-right: 50px;
  font-size: 20px;
}
</style>
