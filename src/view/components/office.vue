<template>
  <div>
    <div>
      <Form ref="office" :model="office"method="post" inline>
      <Input type="text" v-model="office.o_name" placeholder="添加科室" name="name" style="width:1000px">
        <Icon type="ios-person-outline" slot="prepend"></Icon>
      </Input>
      <Button type="primary" @click="submit()" class="addoffice">确定添加</Button>
      </Form>    
    </div>
    <Table border :columns="columns7" :data="data6"></Table>
    <Card v-if="modal2" id="form">
      <form method="post">
        <!-- <i-input :value.sync="formItem.input" name="title" v-show="false"></i-input> -->
        <div id="delete">确认删除？</div>
        <input type="button" value="确认" id="sub" @click="yes()">
        <input type="button" value="取消" @click="modal2=false" id="but">
      </form>
    </Card>
  </div>
</template>


<script>
import PasteEditor from '_c/paste-editor'
// import { getIllData } from '@/api/data'
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
          title: '科室名称',
          key: 'ill_title',
          width:1000,
          render: (h, params) => {
            return h('div', [
              h('Icon', {
                props: {
                  type: 'person'
                }
              }),
              h('strong', params.row.o_name)
            ]);
          }
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
          ]);
          }
        }
      ],
      data6: [

      ],
      office: {
        o_name:''
      },
      modal1: false,
      modal2: false,
      formItem: {
        input: '',
        select: '',
        radio: 'male',
        checkbox: [],
        switch: true,
        date: '',
        time: '',
        slider: [20, 50],
        textarea: '',
        iid:'',
        oid:''
      },
    }
  },
  created() {
    this.get();
  },
  activated(){
    this.get();
  },
  methods: {
    infor1(idx){
      this.formItem.input=this.data6[idx].ill_title;
      this.formItem.textarea=this.data6[idx].ill_content;
      this.formItem.oid=this.data6[idx].oid;
      this.modal2=true;
    },
    get(){
      axios({
        method: 'get',
        url: 'http://localhost/zyy/doctor/getoffice'
      }).then((res) => {
        this.data6 = res.data.data;
      })
    },
    yes(){
      this.modal2=false;
      axios({
        url: 'http://localhost/zyy/doctor/deloffice',
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
    show (index) {
      this.go(index);
    },
    submit () {
      alert('添加成功')
      axios({
        url: 'http://localhost/zyy/doctor/addoffice',
        method: 'post',
        data: this.office,
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
        console.log(this.office)
      })
    }
  }
  
}
</script>

<style lang="less">
#card{
  text-align: center;
  width: 1000px;
  height: 600px;
  position: absolute;
  left: 260px;
  top: 100px;
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
.addoffice{
  position: relative;
  left:1005px;
  top:-30px;
}
</style>
