<template>
<div>
        <h3 style="display:inline;">科室：</h3>
        <Select v-model="keshiRoom" clearable style="width:200px">
            <Option v-for="item in keShi" :value="item.value" :key="item.value">{{ item.label }}</Option>
        </Select>
        <h3 style="display:inline;">医生：</h3>
        <Select v-model="model8" clearable style="width:200px">
            <Option v-for="item in doctor" :value="item.value" :key="item.value">{{ item.label }}</Option>
        </Select>
        <Button type="primary" icon="ios-search" style="margin-left:100px">Search</Button>
        <Button type="primary" style="margin-left:10px">创建病例</Button>
        
        <Table border :columns="columns7" :data="cases"></Table>


        <card 
      v-show="modal1"
      title="病例详情" id="card">
      <div id="div">
        <i-button type="primary" @click="goback" id="back">
            &lt;后退
        </i-button>
        <!-- 表单 -->
        <i-form :model="formItem" :label-width="80" method="post" action="http://localhost/zyy/Cased/updatacase">
          
          <Form-item label="病例名称">
            <i-input v-model="formItem.case_name" placeholder="请输入"  name="casename"></i-input>
          </Form-item>
          <Form-item label="病例时间">
            <i-input v-model="formItem.case_time" name="casetime" :autosize="{minRows: 2,maxRows: 8}" placeholder="请输入..."></i-input>
          </Form-item>

          <h3>诊断结果：</h3>
          <editor ref="editordia" v-model="formItem.case_diagnosis"/> 
          <h3>病例描述：</h3>
          <editor ref="editordes" v-model="formItem.case_describe"/> 
          <h3>医嘱：</h3>
          <editor ref="editoradv" v-model="formItem.case_advice"/>         
          <input type="text" v-model="formItem.case_id" name="caid" v-show="true">
          <Button type="primary" @click="submit()">保存更改</Button>
        
        </i-form>
    </div>
    </card>

    </div>
</template>
<script>
import axios from 'axios'
import Editor from '_c/editor'
// 这里是为了 php 可以收到 post 的值
var qs = require('qs');
export default {
components: {
    Editor
  },
  data () {
    return {
      formItem: {
          
      },
      modal1:false,
      
      columns7: [
        {
          title: '病例名称',
          key: 'case_name',
          render: (h, params) => {
            return h('div', [
              h('Icon', {
                props: {
                  type: 'person'
                }
              }),
              h('strong', params.row.case_name)
            ])
          }
        },
        {
          title: '科室',
          key: 'case_describe',
        },
        {
          title: '医生',
          key: 'case_advice'
        },
        {
          title: 'handle',
          key: 'handle',
          width: 150,
          align: 'center',
          render: (h, params) => {
            return h('div', [
                h('Button', {
                    props: {
                        type: 'primary',
                        size: 'small'
                    },
                    on: {
                        click: () => {
                            
                        }
                    }
                }, '查看'),
                h('Button', {
                    props: {
                        type: 'primary',
                        size: 'small',
                        
                    },
                    style: {
                        marginRight: '5px',
                        to: '/bingli'
                    },
                    on: {
                        click: () => {
                            this.showEdit(params.index);
                        }
                    }
                }, '编辑')
            ]);
          }
        }
      ],
      cases: [
      ],
      keShi: [
        {
            value: '科室1',
            label: '科室1'
        },
        {
            value: '科室2',
            label: '科室2'
        },
        {
            value: '科室3',
            label: '科室3'
        },
        {
            value: '科室4',
            label: '科室4'
        }
      ],

        doctor: [
            {
                value: '医生1',
                label: '医生1'
            },
            {
                value: '医生2',
                label: '医生2'
            },
            {
                value: '医生3',
                label: '医生3'
            },
            {
                value: '医生4',
                label: '医生4'
            },
        ],
        model8: '',
        keshiRoom: '',
        case_advice:'',
        case_describe:'',
        case_name:'',
        case_id:[],
        casesall: {

        },
    }
  },
  created () {
    axios({
            url: 'http://localhost/zyy/Cased/getcase',
            method: 'get'
        }).then(res => {
            console.log(res.data);
            this.casesall = res.data;
            let arr = [];
            for(let i =0; i < res.data.length;i ++) {
             let cases = res.data[i];
            arr.push({ 
                case_name : cases.case_name,
                case_advice : cases.case_advice,
                case_describe : cases.case_describe,
                case_id : cases.case_id,
                })
            }
            this.cases = arr;
        }).catch(res=>{
            console.log('fail');
        });
  },
  methods: {
      submit() {
        axios({
            url: 'http://localhost/zyy/Cased/updatacase',
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
            console.log(res)
        }).catch(err => {
            console.log(err)
        })
        // let recaseid = {
        //     case_id : idx+1,
        // }
        // axios.post(`http://localhost/zyy/Cased/updatacase`,
        //         qs.stringify(recaseid))
        //     .then(res=>{
        //         if(res.data){
        //             alert('更新成功');
        //         }
        //         console.log('success', res.data);
        //     }).catch(res=>{
        //         console.log('fail');
        //     });
      },
    goback() {
      this.modal1=false;
    },
    showEdit(idx) {
      
        const caseid = {
            case_id : idx+1,
        }
        console.log('data'+caseid.case_id);
        //   console.log(idx); qs.stringify(caseid)
        console.log(qs.stringify(caseid));
        
      axios.post('http://localhost/zyy/Cased/get_case_id',qs.stringify(caseid))
      .then(res => {
          console.log(res.data);
          let formItems = res.data;
           this.formItem.case_name = formItems.case_name;
           this.formItem.case_time = formItems.case_time;
           this.formItem.case_id = formItems.caseid;
           this.formItem.case_advice = formItems.case_advice;
           this.formItem.case_describe = formItems.case_describe;
           this.formItem.case_diagnosis = formItems.case_diagnosis;
        // editor
          this.$refs.editoradv.setHtml(this.formItem.case_advice);
          this.$refs.editordes.setHtml(this.formItem.case_describe);
          this.$refs.editordia.setHtml(this.formItem.case_diagnosis);

          this.modal1=true;
      }).catch(res => {
          console.log('获取当前数据失败');
      })
    }
  }
}
</script>
<style>
#card{
  text-align: center;
  width: 1000px;
  height: 1000px;
  position: absolute;
  left: 260px;
  top: 0px;
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
</style>
