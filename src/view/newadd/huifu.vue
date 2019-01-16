<template>
  <div>
      <div class="neirong">
        <h3 style="text-align: left">病例名称：</h3>
        <h3 style="text-align: left">病例时间：</h3>
        <h3 style="text-align: left">诊断结果:</h3>
        <h3 style="text-align: left">病例描述：</h3>
        <h3 style="text-align: left">医嘱：</h3>
          <br>
          <br>
          <h2 style="text-align: left">留言</h2>
          <br>
          <form action="">
            <Input v-model="value" placeholder="Enter something..." style="width: 300px" />
            <Button type="primary">提交</Button>
          </form>
          <br>
          <br>
          <div>
            <div class="liuyan">
              <div class="liuyan_left">
                <span style="text-align:center;display: inline-block">&nbsp&nbsp&nbsp用户名</span>
              </div>
              <div class="liuyan_right">
                &nbsp&nbsp2018.08.02
                <a href="" class="a1">删除</a>
                <br><br>
               希望任医生能够看见这条留言，帮助和治疗更多有肺部疾病的患者，为他们送去福音。
              </div>
            </div>
            <hr>
            <div class="liuyan">
              <div class="liuyan_left">
                <span style="text-align:center;display: inline-block">&nbsp&nbsp&nbsp用户名</span>
              </div>
              <div class="liuyan_right">
                &nbsp&nbsp2018.08.02
                <a href="" class="a1">删除</a>
                <br><br>
                希望任医生能够看见这条留言，帮助和治疗更多有肺部疾病的患者，为他们送去福音。
              </div>
            </div>
            <hr>
            <div class="liuyan">
              <div class="liuyan_left">
                <span style="text-align:center;display: inline-block">&nbsp&nbsp&nbsp用户名</span>
              </div>
              <div class="liuyan_right">
                &nbsp&nbsp2018.08.02
                <a href="" class="a1">删除</a>
                <br><br>
               希望任医生能够看见这条留言，帮助和治疗更多有肺部疾病的患者，为他们送去福音。
              </div>
            </div>
            <hr>
            <div class="liuyan">
              <div class="liuyan_left">
                <span>用户名</span>
              </div>
              <div class="liuyan_right">
                &nbsp&nbsp2018.08.02
                <a href="" class="a1">回复</a>
                <br><br>
                希望任医生能够看见这条留言，帮助和治疗更多有肺部疾病的患者，为他们送去福音。
              </div>
            </div>
            <hr>
          </div>
        </div>

      </div>
    </div>
  </div>


</template>

<script>

  export default {
    methods: {
    //   fanhui(){
    //     this.$router.push(`/Social/`);
    //   }
    }
  }
</script>

<style scoped>
  .fabuzhe{
    margin-top: 100px;
    margin-left: 30%;
    text-align: center;
    height: 150px;
    width:100px ;
    display: inline-block;
    color: #515a6e;
  }
  .touxiang{
    height: 100px;
    width: 100px;
    border-radius: 50px;
    border: solid 1px #808695;
  }
  .fabuzhe span{
    font-size: 20px;
  }
  .chufa span{
    padding-top: 10px;
  }
  .tuji img{
    width: 60%;
    margin-left: 20%;
    height:35%;

  }
  .neirong p{
      font-size: 16px;
    color: #515a6e;
  }
  h3{
    width: 100%;
    height: 50px;
    text-align: center;
  }
  .liuyan{
    height:80px;
    width: 100%;
    overflow: hidden;
    color: #515a6e;
  }
  .liuyan_left{
    display: inline-block;
    width: 15%;
  }
  .liuyan_right{
    display: inline-block;
    /* float: right; */
    width: 83%;
    height: 80%;
    padding-top: 0px;
    position: relative;
    color: #515a6e;
  }
  .liuyan_right a{
    text-decoration: none;
    color:#ff9900;
    position: absolute;
    right: 0px;
  }
  .liuyan_right .a1{
    position: absolute;
    right: 35px;

  }
  </style>
