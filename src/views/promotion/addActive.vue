<template>
	<div class="redact addActive" align="center">
		<el-breadcrumb separator-class="el-icon-arrow-right" style="font-size: 14px">
			<el-breadcrumb-item :to="{ path: '/' }">营销</el-breadcrumb-item>
			<el-breadcrumb-item :to="{ path: '/activeList' }">活动列表</el-breadcrumb-item>
			<el-breadcrumb-item>活动添加</el-breadcrumb-item>
		</el-breadcrumb>
    <el-tabs v-model="activeName" @tab-click="handleClick" class="active">
      <el-tab-pane label="限时优惠" name="first">
        <div style="margin-top: 30px;float: left">
          <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
            <el-form-item label="活动标题：" prop="name">
              <el-input v-model="ruleForm.name" plea placeholder="活动标题"></el-input>
            </el-form-item>
            <el-form-item label="开始时间：" prop="date1">
              <el-date-picker
                v-model="ruleForm.date1"
                type="datetime"
                placeholder="选择日期时间">
              </el-date-picker>
            </el-form-item>
            <el-form-item label="结束时间：" prop="date1">
              <el-date-picker
                v-model="ruleForm.date2"
                type="datetime"
                placeholder="选择日期时间">
              </el-date-picker>
            </el-form-item>
            <el-form-item label="规则：" prop="num">
              <span style="float: left;">限购数量</span>
              <el-input v-model="ruleForm.num" class="num"></el-input>
            </el-form-item>
            <el-form-item label="活动描述：" prop="desc">
              <el-input type="textarea" v-model="ruleForm.desc"></el-input>
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="submitForm('ruleForm')">提交</el-button>
            </el-form-item>
          </el-form>
        </div>
      </el-tab-pane>
      <!--//满减优惠-->
      <el-tab-pane label="满减优惠" name="second"><div style="margin-top: 30px;float: left">
        <el-form :model="ruleForm2" :rules="rules2" ref="ruleForm2" label-width="100px" class="demo-ruleForm">
          <el-form-item label="活动标题：" prop="name">
            <el-input v-model="ruleForm2.name" plea placeholder="活动标题"></el-input>
          </el-form-item>
          <el-form-item label="开始时间：" prop="date1">
            <el-date-picker
              v-model="ruleForm2.date1"
              type="datetime"
              placeholder="选择日期时间">
            </el-date-picker>
          </el-form-item>
          <el-form-item label="结束时间：" prop="date1">
            <el-date-picker
              v-model="ruleForm2.date2"
              type="datetime"
              placeholder="选择日期时间">
            </el-date-picker>
          </el-form-item>
          <el-form-item label="试用范围：" prop="resource">
            <el-radio-group v-model="ruleForm2.resource">
              <el-radio :label="1">全平台</el-radio>
              <el-radio :label="2">部分商品</el-radio>
            </el-radio-group>
          </el-form-item>
          <el-form-item label="规则：" prop="money" class="item_ctive">
           <ul class="youhui">
             <li>
               <span>订单满</span>
               <div>
                 <em v-if="ruleForm2.money[0].all!=''">￥</em>
                 <el-input v-model="ruleForm2.money[0].all"></el-input>
               </div>
               <span>减</span>
               <div >
                 <em v-if="ruleForm2.money[0].jian!=''">￥</em>
                 <el-input v-model="ruleForm2.money[0].jian" @blur="check_money(0)"></el-input>
               </div>
               <p v-if="check[0].check">优惠必须小于购买价格</p>
             </li>
             <li>
               <span>订单满</span>
               <div>
                 <em v-if="ruleForm2.money[1].all!=''">￥</em>
                 <el-input v-model="ruleForm2.money[1].all"></el-input>
               </div>
               <span>减</span>
               <div>
                 <em v-if="ruleForm2.money[1].jian!=''">￥</em>
                 <el-input v-model="ruleForm2.money[1].jian" @blur="check_money(1)"></el-input>
               </div>
               <p v-if="check[1].check">优惠必须小于购买价格</p>
             </li>
             <li>
               <span>订单满</span>
               <div>
                 <em v-if="ruleForm2.money[2].all!=''">￥</em>
                 <el-input v-model="ruleForm2.money[2].all" ></el-input>
               </div>
               <span>减</span>
               <div>
                 <em v-if="ruleForm2.money[2].jian!=''">￥</em>
                 <el-input v-model="ruleForm2.money[2].jian" @blur="check_money(2)"></el-input>
               </div>
               <p v-if="check[2].check">优惠必须小于购买价格</p>
             </li>
             <li>
               <span>最高减</span>
               <div>
                 <em v-if="ruleForm2.money[3].all!=''">￥</em>
                 <el-input v-model="ruleForm2.money[3].all"></el-input>
               </div>

             </li>

           </ul>
            <p v-if="item_true" style="float: left;margin: 0;color: #f56c6c;font-size: 14px;line-height: 0">至少一项满减活动</p>
           <!-- <span style="float: left;">限购数量</span>
            <el-input v-model="ruleForm2.name" class="num"></el-input>-->
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="submitForm('ruleForm2')">提交</el-button>
          </el-form-item>
        </el-form>
      </div>
      </el-tab-pane>
    </el-tabs>


	</div>

</template>

<script>
  import {api_active} from "@/api/api.js"//export class api_Chat{}中的总名称
  let apiActive=new api_active()
export default {
  data() {
    return {
      ruleForm: {
        name: '',//活动标题
        date1: '',//开始时间
        date2: '',//结束时间
        desc: '',
        num:''
      },
      item_true:false,//控制满减活动
      item_theck:false,
      activeName: 'first',
      rules: {
        name: [
          { required: true, message: '请输入活动标题', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
        ],
        date1: [
          { type: 'date', required: true, message: '请选择日期', trigger: 'change' }
        ],
        date2: [
          { type: 'date', required: true, message: '请选择时间', trigger: 'change' }
        ],
        desc: [
          { required: true, message: '请填写活动形式', trigger: 'blur' }
        ],
        num:[ { required: true, message: '请输入限购数量', trigger: 'blur' }]
      },
     ruleForm2: {
         name: '',//活动标题
         date1: '',//开始时间
         date2: '',//结束时间
         resource:1,//应用平台
         money:[
           {all:'',jian:''},
           {all:'',jian:''},
           {all:'',jian:''},
           {all:'',}
         ]//满减规则
     },
      rules2: {
        name: [
          { required: true, message: '请输入活动标题', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
        ],
        date1: [
          { type: 'date', required: true, message: '请选择日期', trigger: 'change' }
        ],
        date2: [
          { type: 'date', required: true, message: '请选择时间', trigger: 'change' }
        ],
        resource: [
          { required: true, message: '请选择活动资源', trigger: 'change' }
        ],

      },
      check:[
        {check:false},
        {check:false},
        {check:false},
      ]


    };
  },
  methods: {
    submitForm(formName) {

    this.$refs[formName].validate((valid) => {
        if (valid) {
          if(formName=='ruleForm2'){
            for(var i=0;i<this.ruleForm2.money.length;i++){
              var obj=this.ruleForm2.money[i]
              for(var item in obj){
                if(obj[item]!='' ){
                  this.item_theck=true
                  break;
                }
              }
            }
            console.log( this.item_theck)
            if(this.item_theck){
              this.item_true=false
            }else{

              this.item_true=true
              return
            }
            //满减优惠
            var all_arr=[]
            for (var i=0;i<this.ruleForm2.money.length-1;i++){
              var arr=[]
              for(var item in this.ruleForm2.money[i]){
                if(this.ruleForm2.money[i][item]!=''){

                  arr.push(this.ruleForm2.money[i][item])
                }
              }
              if(arr.length>0){
                all_arr.push(arr)
              }
            }
           let newarr=[]
            all_arr.forEach(x=>{
              newarr.push({full_money:x[0],subtract_money:x[1]})
            })
            let param={
              title:this.ruleForm2.name,
              types:2,
              status:1,
              add_time:Number(String(this.ruleForm2.date1.getTime()).slice(0,10)),
              end_time:Number(String(this.ruleForm2.date2.getTime()).slice(0,10)),
              tall_money:this.ruleForm2.money[3].all,
              scope:this.ruleForm2.resource,
              subtract_rule:newarr,
              content:''
            }
            this.add(param,'满减优惠添加成功')
          }else if(formName=='ruleForm') {
            let param={
              title:this.ruleForm.name,
              types:1,
              status:1,
              add_time:Number(String(this.ruleForm.date1.getTime()).slice(0,10)),
              end_time:Number(String(this.ruleForm.date2.getTime()).slice(0,10)),
              tall_money:'',
              scope:'',
              subtract_rule:'',
              content:this.ruleForm.desc,
              purchase_num:this.ruleForm.num
            }
            this.add(param,'限时优惠添加成功')
          }
        } else {
          return false;
        }
      });

    },
    //判断优惠价格是否合理
    check_money(num){
      if(this.ruleForm2.money[num].jian!='' && this.ruleForm2.money[num]!=''){

        if(Number(this.ruleForm2.money[num].jian)>Number(this.ruleForm2.money[num].all) || Number(this.ruleForm2.money[num].jian)==Number(this.ruleForm2.money[num].all)){
          this.check[num].check=true
        }else {
          this.check[num].check=false
        }
      }


    },
    add(param,str){
      apiActive.discount(param).then(res => {
        console.log(res)
        if(res.cscode==0){
          this.$message({type: 'success',message:str, showClose: true,})
          this.ruleForm2.name=''
          this.ruleForm2.date1=''
          this.ruleForm2.date2=''
          this.ruleForm2.resource=1
          this.ruleForm2.money[0].all=''
          this.ruleForm2.money[0].jian=''
          this.ruleForm2.money[1].all=''
          this.ruleForm2.money[1].jian=''
          this.ruleForm2.money[2].all=''
          this.ruleForm2.money[2].jian=''
          this.ruleForm2.money[3].all=''
          this.ruleForm.name=''
          this.ruleForm.date1=''
          this.ruleForm.date2=''
          this.ruleForm.num=''
          this.ruleForm.desc=''
        }else {
          this.$message.error(res.data.msg)
        }
      })
    },
    handleClick(tab, event) {
      console.log(tab, event);
    }
  },

 }
</script>

<style scoped>

  .el-form{
    width: 25rem!important;
  }
  .shengs{
    display: inline-block;
  }
  .el-input{
    width: 100% !important;
  }

/*所有的div*/
.chare1{white-space:nowrap; padding-top: 0.5rem;}

.el-input__inner{
	height: 55px;
}


.el-form-item__content{
	white-space: nowrap;
	font-size: 20px;
}
.el-form-item {
    margin-bottom: 20px;
}
/*按钮*/


@import url("../../assets/css/UserRedact.css");
	.el-form-item__content {font-size: 20px;}
  .active ul,.active ul li{margin: 0;padding: 0}
  .active ul li{list-style: none;display: flex;margin-bottom: 10px}
  .addActive ul li>div{position: relative}
  .addActive ul li>div em{font-style: normal;position: absolute;left: 13px;z-index: 1111;color: #c0c4cc}
</style>
<style>
  .num span{float: left}
 .redact .num .el-input__inner{
    width: 70px!important;
    float: left;
    margin-left: 80px;
    margin-top: -65px;
  }
  .el-radio-group{float:left;margin-top: 15px }
  .active .el-tabs__header{overflow: hidden;height: 30px;margin-top: 20px}
  .active ul li .el-input__inner{ width: 70px!important;margin: 0 10px}
  .active ul li .el-input{width: 80px!important}
  .active ul li span:nth-of-type(2){margin-left: 10px}
  .active .item_ctive .el-form-item__label:before {
    content: "*";color: red;margin-right: 4px;
  }
  .addActive .el-breadcrumb{display: block!important;}
  .addActive ul.youhui li{
    position: relative;
    height: 60px;
  }
  .addActive ul.youhui li p{
    position: absolute;
    top: 20px;
    color: #f56c6c;
  }
</style>
