<template>
  <div>
    <Form class="form1">
      <FormItem prop="name" class="formitem2">
          <h3 style="display:inline;">科室：</h3>
        <Select v-model="search.oid" clearable style="width:200px" class="select">
            <Option v-for="item in office" :value="item.oid" :key="item.oid">{{ item.o_name }}</Option>
        </Select>
      </FormItem>
      <FormItem prop="type" class="formitem2">
        <h3 style="display:inline;">医生：</h3>
        <Select v-model="search.did" clearable style="width:200px" class="select">
            <Option v-for="item in doc" :value="item.did" :key="item.did">{{ item.doctor_name }}</Option>
        </Select>
      </FormItem>
      <FormItem class="formitem2">
        <Button type="primary" @click="sear()">选择科室医生搜索病例</Button>
      </FormItem>
    </Form>
    <Table border :columns="columns7" :data="data6"></Table>
    <card 
      v-show="modal1"
      title="病例详情页" id="card">
      <div id="div">
        <i-button type="primary" @click="goback" id="back">
            &lt;后退
        </i-button>
        <i-form :model="formItem" :label-width="80" method="post">
          <Form-item label="病例名称">
            <i-input v-model="formItem.c_name" placeholder="" name="title"></i-input>
          </Form-item>
          <!-- <Form-item label="病症详情">
            <i-input v-model="formItem.textarea" type="textarea" name="content" :autosize="{minRows: 2,maxRows: 8}" placeholder="请输入..."></i-input>
          </Form-item> -->
          <h3 style="display:inline;">病情诊断：</h3>
          <editor ref="editor" v-model="formItem.c_diagnosis"/>
          <h3 style="display:inline;">病情描述：</h3>
          <editor ref="editor" v-model="formItem.c_describe"/>
          <h3 style="display:inline;">解决方案：</h3>
          <editor ref="editor" v-model="formItem.c_solve"/>
          <h3 style="display:inline;">注意事项：</h3>
          <editor ref="editor" v-model="formItem.c_attention"/>
          <input type="button" id="btnl" @click="info" value="保存更改">
        </i-form>
    </div>
    </card>
    <Card v-if="modal2" id="form">
      <form method="post">
        <i-input :value.sync="formItem.input" name="title" v-show="false"></i-input>
        <i-input :value.sync="formItem.textarea" name="content" v-show="false"></i-input>
        <div id="delete">确认删除？</div>
        <input type="button" value="确认" id="sub" @click="yes()">
        <input type="button" value="取消" @click="modal2=false" id="but">
      </form>
    </Card>
    <Card v-if="modal3" id="card">
      <div id="div">
        <i-button type="primary" @click="goback2" id="back">
            &lt;后退
        </i-button>
        <ul v-for="(value,index) in getliu" :key="value.index" class="liu">
          <li>{{value.d_time}}  {{value.d_sign}}</li>
          <li>{{value.d_content}}</li>
        </ul>
        <i-form :model="liu" :label-width="80" method="post">
          <Form-item label="发布留言">
            <i-input v-model="liu.add" placeholder="" name="title"></i-input>
          </Form-item>
          <Button type="primary" @click="adddis()" class="but">点击发布</Button>
        </i-form>
    </div>
    </Card>
    
    
  </div>
</template>


<script>
import PasteEditor from '_c/paste-editor'
import { getIllData } from '@/api/data'
import Editor from '_c/editor'
import axios from "axios"
export default {
  name: 'illclass_details_page',
  components: {
    Editor
  },
  data () {
    return {
      columns7: [
        {
          title: '就诊人姓名',
          key: 'r_name',
          width:200,
          render: (h, params) => {
            return h('div', [
              h('Icon', {
                props: {
                  type: 'person'
                }
              }),
              h('strong', params.row.r_name)
            ]);
          }
      },
        {
          title: '科室',
          key: 'o_name',
          align: 'center',
        },
        {
          title: '医生',
          key: 'doctor_name',
          align: 'center',
        },
        {
          title: '病情描述',
          key: 'c_describe',
          align: 'center',
        },
        {
          title: '就诊日期',
          key: 'r_date',
          align: 'center',
        },
        {
          title: 'Action',
          key: 'action',
          width: 200,
          align: 'center',
          render: (h, params) => {
          return h('div', [
            h('Button', {
              props: {
                type: 'primary',
                size: 'small'
              },
              style: {
                marginRight: '5px'
              },
              on: {
                click: () => {
                  this.show(params.index)
                }
              }
            }, '编辑'),
            h('Button', {
              props: {
                type: 'primary',
                size: 'small'
              },
              style: {
                marginRight: '5px'
              },
              on: {
                click: () => {
                  this.infor1(params.index)
                }
              }
            }, '删除'),
            h('Button', {
              props: {
                type: 'primary',
                size: 'small'
              },
              style: {
                marginRight: '5px'
              },
              on: {
                click: () => {
                  this.liuyan(params.index)
                }
              }
            }, '留言'),
          ]);
          }
        }
      ],
      liu:{
        d_pid:'',
        add:'',
        time:'',
        sign:'医生'
      },
      getliu:[

      ],
      data6: [

      ],
      office: {

      },
      doc: {

      },
      search: {
        oid:'',
        did:''
      },
      modal1: false,
      modal2: false,
      modal3:false,
      formItem: {
        c_name:'',
        c_diagnosis:'',
        c_describe:'',
        c_solve:'',
        c_attention:'',
        c_id:''
      },
    }
  },
  created() {
    this.get();
    this.send();
    this.send2();
  },
  activated(){
    this.get();
  },
  methods: {
    go (idx) {
      this.formItem.c_name=this.data6[idx].c_name;
      this.formItem.c_diagnosis=this.data6[idx].c_diagnosis;
      this.formItem.c_describe=this.data6[idx].c_describe;
      this.formItem.c_solve=this.data6[idx].c_solve;
      this.formItem.c_id=this.data6[idx].c_id;
      this.formItem.c_attention=this.data6[idx].c_attention;
      this.modal1=true;
    },
    on () {
      this.$router.push('/illclass/illclass_add_page')
    },
    goback (){
      this.modal1=false;
    },
    goback2 () {
      this.modal3=false;
    },
    sear () {
      // console.log(this.search)
      axios({
        url: 'http://localhost/zyy/user/secase',
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
        this.data6=res.data.data
        console.log(this.data6)
      }).catch(err => {
        console.log(err)
      })
    },
    info () {
      axios({
        url: 'http://localhost/zyy/User/changecase',
        method: 'post',
        data: this.formItem,
        transformRequest: function (obj) {
          var str = []
          for (var p in obj) {
            str.push(encodeURIComponent(p) + '=' + encodeURIComponent(obj[p]))
          }
          return str.join('&')
        }
      }).then(res => {
        this.modal1=false;
        alert('更改成功');
        this.get();
        console.log(res)
      }).catch(err => {
        console.log(err)
      })
    //   console.log(this.formItem)
    },
    infor1(idx){
      this.formItem.c_id=this.data6[idx].c_id;
      this.modal2=true;
    },
    liuyan (idx){
      this.liu.d_pid=this.data6[idx].c_id;
      // console.log(this.data6[idx].d_time);
      axios({
        url: 'http://localhost/zyy/user/sedis',
        method: 'post',
        data: this.liu,
        transformRequest: function (obj) {
          var str = []
          for (var p in obj) {
            str.push(encodeURIComponent(p) + '=' + encodeURIComponent(obj[p]))
          }
          return str.join('&')
        }
      }).then(res => {
        this.getliu=res.data.data
        console.log(this.getliu)
        this.modal3=true;
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
        url: 'http://localhost/zyy/doctor/getoffice'
      }).then((res) => {
        this.office = res.data.data
        console.log(this.office)
      })
    },
    get(){
      // getIllData().then(res => {
      //   this.illdata = res.data.data;
      // }).catch(err => {
      //   console.log(err)
      // })
      axios({
        method: 'get',
        url: 'http://localhost/zyy/User/getcase'
      }).then((res) => {
        this.data6 = res.data.data;
        console.log(this.data6)
      })
    },
    yes(){
      this.modal2=false;
      axios({
        url: 'http://localhost/zyy/user/delcase',
        method: 'post',
        data: this.formItem,
        transformRequest: function (obj) {
          var str = []
          for (var p in obj) {
            str.push(encodeURIComponent(p) + '=' + encodeURIComponent(obj[p]))
          }
          return str.join('&')
        }
      }).then(res => {
        alert('删除成功');
        this.get();
        console.log(res)
      }).catch(err => {
        console.log(err)
      })
    },
    adddis(){
      this.liu.time=new Date(+new Date()+8*3600*1000).toISOString().replace(/T/g,' ').replace(/\.[\d]{3}Z/,'');
      console.log(this.liu);
      axios({
        url: 'http://localhost/zyy/user/adddis',
        method: 'post',
        data: this.liu,
        transformRequest: function (obj) {
          var str = []
          for (var p in obj) {
            str.push(encodeURIComponent(p) + '=' + encodeURIComponent(obj[p]))
          }
          return str.join('&')
        }
      }).then(res => {
        alert('留言成功');
        this.get();
        console.log(res)
      }).catch(err => {
        console.log(err)
      })
    },
    show (index) {
      this.go(index);
    },
  },
  
}
</script>

<style lang="less">
#card{
  text-align: center;
  width: 1000px;
//   height: 1600px;
  position: relative;
//   left: 260px;
  top: -140px;
  z-index: 1;
}
#div{
  text-align: left;
}
#btnl{
  height: 30px;
}

#back{
  position: absolute;
  left: 5px;
  top: 10px;
}
#form{
  text-align: center;
  width: 400px;
  height: 300px;
  background: #ccc;
  position: absolute;
  top: 100px;
  left: 500px;
  z-index: 1;
}
#sub{
  width: 50px;
  height: 30px;
  position: absolute;
  bottom: 20px;
  left: 80px;
}
#but{
  width: 50px;
  height: 30px;
  position: absolute;
  bottom: 20px;
  right: 80px;
}
#delete{
  font-size: 30px;
  position: relative;
  top: 100px;
}
.form1{
  position: relative;
  height: 50px;
  // float: left;
}
.formitem2{
  width: 300px;
  position: absolute;
  // height: 30px;
  // overflow: hidden;
  // float: left;
  // top: 0;
}
.formitem2:nth-child(2){
  left: 300px;
}
.formitem2:nth-child(3){
  left: 600px;
}

.select{
  position: relative;
  // z-index: 100000;
}
.liu{
  list-style: none;
  position: relative;
  margin-left: 100px;
  margin-top: 5px;
}
.but{
  position: relative;
  margin-left: 400px;
}
</style>
