<template>
    <div class="searchoutbox">
        <div class="leftbox">
               <div class="leftinbox1">
                     <div class="inleftbox">   
                      <div class="leftin1">   
                        <div class="wfont"> <el-radio v-model="radio" :label="0" class="pdw">全天水量</el-radio></div>
                        <div> <input type="text" id="input1" placeholder="输入全天水量" v-model="DailyWater" v-bind:disabled="radio==0?false:true"></div>
                        <div class="wdw">m³/d</div>
                     </div>
                    <div class="leftin2">
                      <div class="pfont"><el-radio v-model="radio" :label="1" class="pdw1">瞬时水量</el-radio></div>
                      <input type="text" name="" id="input2" v-model="instantwater" placeholder="输入瞬时水量" v-bind:disabled="radio==1?false:true">
                      <div class="pdw">m³/h</div>
                    </div>
                       <div class="leftin3">
                         <div class="pfont1">时段</div>
                          <el-select v-model="value" placeholder="请选择">
                            <el-option
                              v-for="item in options"
                              :key="item.value"
                              :label="item.label"
                              :value="item.value">
                            </el-option>
                          </el-select>
                    </div>
                    <div class="leftin4">
                      <div class="pfont">气温范围</div>
                      <input type="text" name="" id="input2" v-model="lowtempreture" placeholder="输入温度">
                      <div class="pdw">℃</div>
                    </div>
                    <div class="leftin5">
                      <div class="pfont5">至</div>
                      <input type="text" name="" id="input2" v-model="hightempreture" placeholder="输入温度">
                      <div class="pdw">℃</div>
                    </div>
                </div>
                <div class="inrightbox">
                     <div class="searchleftin3" @click="getInfo">查询</div>
                </div>
               </div>
              
          </div>
            <ul class="rightbox tabs">
            <li class="rightboxin1 rightboxin" v-for="(item,index) in tabsParam" :key="item.tabsParam"
                 @click="toggleTabs(index)"
                  :class="{active:index===nowIndex}">{{item}}
               </li>
            </ul>
    </div>
</template>
<script>
/* import {mapActions} from "vuex"; */
import Bus from "@/bus.js";
import urlClass from "@/components/js/UrlClass";
export default {
  name: "Search",
  data() {
    return {
      radio: 0,
      radioshow: false,
      DailyWater: "",
      instantwater: "",
      lowtempreture: "",
      hightempreture: "",
      tabsParam: [
        "历年高峰供水调度方案",
        "历年节假日供水调度方案",
        "历年水厂停产减产调度方案",
        "历年极端气候调度方案",
        "历年爆管事故调度方案"
      ],
      nowIndex: 0,
      isShow: false,
      options: [
        {
          value: "0",
          label: "工作日"
        },
        {
          value: "1",
          label: "节假日"
        }
      ],
      value: "",
      object: {},
      objectdata0:{},
      objecharts1data1:{},
      WaterfactorynameParam:[],
      wfctorynowIndex:0,
      diaodulisttablecansu:{}
    };
  },
  mounted(){
    let self=this;
     Bus.$on('Waterfactoryname', (e) => {
         self.WaterfactorynameParam = e
       })
        Bus.$on('WaterfactorynowIndex', (e) => {
         self.wfctorynowIndex = e
       })
     Bus.$emit("fanganlibiaosuoyin", self.nowIndex);
  },
  methods: {
    //切换tab项
    toggleTabs(index) {
      this.nowIndex = index;
      Bus.$emit("fanganlibiaosuoyin", this.nowIndex);
     /*  console.log(this.nowIndex); */
    },

    getInfo() {
      var _this = this;
      Bus.$emit("radionumdata", _this.radio);
      var FlowType;
      var DailyWaterSupply;
      var DateType;
      var MinTemp;
      var MaxTemp;
      var SchemeNo;
      if (_this.radio == 0) {
        FlowType = _this.radio;
        DailyWaterSupply = _this.DailyWater;
        DateType = _this.value;
        MinTemp = _this.lowtempreture;
        MaxTemp = _this.hightempreture;
        SchemeNo = _this.nowIndex;
        var obj = {
          FlowType,
          DailyWaterSupply,
          DateType,
          MinTemp,
          MaxTemp,
          SchemeNo
        };
        _this.object = obj;
      } else if (_this.radio == 1) {
        FlowType = _this.radio;
        DailyWaterSupply = _this.instantwater;
        SchemeNo = _this.nowIndex;
        obj = { FlowType, DailyWaterSupply, SchemeNo };
        _this.object = obj;
      }
      _this.ec6();
    },
     ec11() {
      var _this = this;
          var WaterworksNo;
          var objecharts1=_this.objectdata0
          objecharts1.WaterworksNo ="笔架山水厂"
          _this.objecharts1data1=objecharts1
      _this.$axios
        .post(urlClass.axiosUrYA + "GetWaterquantity", JSON.stringify(_this.objecharts1data1), {
          headers: { "Content-Type": "application/json;" }
        })
        .then(res => {
          var _this = this;
          Bus.$emit("info11", res.data);
          console.log(res)
        })
        .catch(error => {
          console.log(error);
        });
    },
    ec12() {
      var _this = this;
          var WaterworksNo;
          var objecharts1=_this.objectdata0
          objecharts1.WaterworksNo ="笔架山水厂"
          _this.objecharts1data1=objecharts1
      _this.$axios
        .post(urlClass.axiosUrYA + "GetOutPressure", JSON.stringify(_this.objecharts1data1), {
          headers: { "Content-Type": "application/json;" }
        })
        .then(res => {
          var _this = this;
          Bus.$emit("info12", res.data);
        })
        .catch(error => {
          console.log(error);
        });
    },
     ec13() {
      var _this = this;
          var WaterworksNo;
          var objecharts1=_this.objectdata0
          objecharts1.WaterworksNo ="笔架山水厂"
          _this.objecharts1data1=objecharts1
      _this.$axios
        .post(urlClass.axiosUrYA + "GetWaterPoolLevel", JSON.stringify(_this.objecharts1data1), {
          headers: { "Content-Type": "application/json;" }
        })
        .then(res => {
          var _this = this;
          Bus.$emit("info13", res.data);
        })
        .catch(error => {
          console.log(error);
        });
    },
      ec14() {
      var _this = this;
          var WaterworksNo;
          var objecharts1=_this.objectdata0
          objecharts1.WaterworksNo ="笔架山水厂"
          _this.objecharts1data1=objecharts1
      _this.$axios
        .post(urlClass.axiosUrYA + "GetWaterUnitConsumption", JSON.stringify(_this.objecharts1data1), {
          headers: { "Content-Type": "application/json;" }
        })
        .then(res => {
          var _this = this;
          Bus.$emit("info14", res.data);
        })
        .catch(error => {
          console.log(error);
        });
    },
    ec15() {
      var _this = this;
          var WaterworksNo;
          var objecharts1=_this.objectdata0
          objecharts1.WaterworksNo ="笔架山水厂"
          _this.objecharts1data1=objecharts1
      _this.$axios
        .post(urlClass.axiosUrYA + "GetWaterTotalEfficiency", JSON.stringify(_this.objecharts1data1), {
          headers: { "Content-Type": "application/json;" }
        })
        .then(res => {
          var _this = this;
          Bus.$emit("info15", res.data);
        })
        .catch(error => {
          console.log(error);
        });
    },
     ec16() {
      var _this = this;
          var WaterworksNo;
          var objecharts1=_this.objectdata0
          objecharts1.WaterworksNo ="笔架山水厂"
          _this.objecharts1data1=objecharts1
      _this.$axios
        .post(urlClass.axiosUrYA + "GetWaterPump", JSON.stringify(_this.objecharts1data1), {
          headers: { "Content-Type": "application/json;" }
        })
        .then(res => {
          var _this = this;
          Bus.$emit("info16", res.data);
        })
        .catch(error => {
          console.log(error);
        });
    },
       ec4() {
      var _this = this;
      _this.$axios
        .post(urlClass.axiosUrYA + "GetPressurePoint", JSON.stringify(_this.objectdata0), {
          headers: { "Content-Type": "application/json;" }
        })
        .then(res => {
          var _this = this;
          Bus.$emit("info4", res.data);
        })
        .catch(error => {
          console.log(error);
        });
    },
    ec5() {
      var _this = this;
      _this.$axios
        .post(urlClass.axiosUrYA + "GetTotalNumber", JSON.stringify(_this.objectdata0), {
          headers: { "Content-Type": "application/json;" }
        })
        .then(res => {
          var _this = this;
          Bus.$emit("info5", res.data);
        })
        .catch(error => {
          console.log(error);
        });
    }, 
    ec6() {
      var _this = this;
      _this.$axios
        .post(
          urlClass.axiosUrYA + "GetSchemeList",
          JSON.stringify(_this.object),
          {
            headers: { "Content-Type": "application/json;" }
          }
        )
        .then(res => {
          var _this = this;
         
           Bus.$emit("info6", res.data);
          var SchemeID=res.data.SchemeList[0].SchemeID;
          var SchemeNo=res.data.SchemeList[0].SchemeNo;
          var SchemeDate=res.data.SchemeList[0].SchemeDate;
          var TotalWaterSupply=res.data.SchemeList[0].TotalWaterSupply;
          var FlowType = _this.radio;
          var obj0={SchemeID,SchemeNo,SchemeDate,TotalWaterSupply,FlowType}
          Bus.$emit("info6tablist", obj0);
          _this.objectdata0=obj0
          var diaodulisttabledata={SchemeNo,SchemeDate};
          _this.diaodulisttablecansu=diaodulisttabledata
          console.log(_this.diaodulisttablecansu)
           _this.ec5();
            _this.ec4();
            _this.ec7();
         _this.ec11();
          _this.ec12();
          _this.ec13()
          _this.ec14()
          _this.ec15() 
          _this.ec16()
        })
        .catch(error => {
          console.log(error);
        });
    },
    handleClick() {
      alert("button click");
    },
       ec7() {
      var _this = this;
     /*  alert("执行了ec7")
      console.log(_this.diaodulisttablecansu) */
      _this.$axios
        .post(urlClass.axiosUrYA + "ManagementDispatch", JSON.stringify(_this.diaodulisttablecansu), {
          headers: { "Content-Type": "application/json;" }
        })
        .then(res => {
          var _this = this;
        /*   alert("ec7")
          console.log(res) */
          Bus.$emit("info7", res.data);
        })
        .catch(error => {
          console.log(error);
        });
    },
  }
};
</script>
<style>
.el-dropdown {
  vertical-align: top;
  width: 120px;
}
.el-dropdown + .el-dropdown {
  margin-left: 15px;
}
.el-icon-arrow-down {
  font-size: 12px;
}
body .el-button.el-button--mini {
  padding: 0 12px;
  height: 28px;
  width: 120px;
}
.el-icon-arrow-down:before {
  margin-left: 30px;
}
</style>
<style lang="scss" scoped>
.searchoutbox {
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: flex-start;
  align-items: center;
  width: 100%;
  height: 166px;
}
.leftbox {
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  flex-direction: column;
  flex-wrap: nowrap;
  justify-content: flex-start;
  align-items: center;
  height: 100%;
  min-width: 1230px;
  width: -moz-calc(100% - 287px);
  width: -webkit-calc(100% -287px);
  width: calc(100% - 287px);
  border-right: 1px #e4e4ec solid;
  /*   background-color: aqua */
}
.leftinbox1 {
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: flex-start;
  align-items: center;
  height: 100%;
  width: 100%;
  /*   background-color: aqua */
}

.inleftbox {
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: flex-start;
  align-items: center;
  height: 100%;
  width: 80%;
  min-width: 1040px;
  margin: 0 20px;
  /*  background-color: aqua */
}
.leftin1 {
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: flex-start;
  align-items: center;
}
.leftin2 {
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: flex-start;
  align-items: center;
  margin-left: 40px;
}
.leftin3 {
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: flex-start;
  align-items: center;
  margin-left: 40px;
  width: 160px;
}
.leftin4 {
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: flex-start;
  align-items: center;
  margin-left: 40px;
}
.leftin5 {
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: flex-start;
  align-items: center;
  margin-left: 40px;
  margin-left: -10px;
}
.inrightbox {
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: center;
  align-items: center;
  height: 100%;
  width: 20%;
  cursor: pointer;
  /*   background-color: aqua */
}

.wfont {
  font-family: "微软雅黑";
  font-size: 14px;
  /*   font-weight: bold; */
  color: #6e7b8b;
  margin-right: 20px;
  width: 60px;
  height: 21px;
  line-height: 21px;
}
.wdw {
  font-family: "微软雅黑";
  font-size: 14px;
  /*   font-weight: bold; */
  color: #6e7b8b;
  margin-left: 10px;
  width: 37px;
  height: 21px;
  line-height: 21px;
}

input {
  /*  width: 432px; */
  width: 5.5vw;
  min-width: 120px;
  height: 34px;
  background-color: #f0f0f1;
  text-align: start;
  /* padding-left: 15px */
}
#input1 {
  margin-left: 10px;
}
#input2 {
  margin-left: -30px;
}
.pfont {
  font-family: "微软雅黑";
  font-size: 14px;
  /*  font-weight: bold; */
  color: #6e7b8b;
  margin-right: 20px;
  width: 90px;
  height: 21px;
  line-height: 21px;
}
.pfont1 {
  font-family: "微软雅黑";
  font-size: 14px;
  /*  font-weight: bold; */
  color: #6e7b8b;
  margin-right: 10px;
  width: 45px;
  height: 21px;
  line-height: 21px;
}
.pfont5 {
  font-family: "微软雅黑";
  font-size: 14px;
  /*  font-weight: bold; */
  color: #6e7b8b;
  margin-right: 20px;
  width: 50px;
  height: 21px;
  line-height: 21px;
}
.pdw {
  font-family: "微软雅黑";
  font-size: 14px;
  /*   font-weight: bold; */
  color: #6e7b8b;
  margin: 0 10px;
  width: 37px;
  height: 21px;
  line-height: 21px;
}
.pdw1 {
  font-family: "微软雅黑";
  font-size: 14px;
  /*   font-weight: bold; */
  color: #6e7b8b;
  /*  margin: 0 10px; */
  width: 37px;
  height: 21px;
  line-height: 21px;
}
input::-webkit-input-placeholder {
  padding-left: 5px;
  color: #acb3ba;
}
input:-moz-placeholder {
  margin-left: 5px;
  color: #acb3ba;
}
input::-moz-placeholder {
  margin-left: 5px;
  color: #acb3ba;
}
input:-ms-input-placeholder {
  margin-left: 5px;
  color: #acb3ba;
}

.searchleftin3 {
  width: 132px;
  height: 48px;
  background-color: #70991f;
  border-radius: 5px;
  font-family: "微软雅黑";
  font-size: 18px;
  color: #e2ebd2;
  text-align: center;
  line-height: 48px;
}
.rightbox {
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  flex-direction: column;
  flex-wrap: nowrap;
  justify-content: center;
  align-items: flex-start;
  height: 100%;
  width: 287px;
  min-width: 158px;
}
.rightboxin {
  font-family: "微软雅黑";
  font-size: 12px;
  font-weight: bold;
  color: #6e7b8b;
  height: 34px;
  line-height: 30px;
  text-align: center;
  width: 100%;
  border-bottom: 1px #e4e4ec solid;
  cursor: pointer;
}

.inrightboxin5 {
  color: #6e7b8b;
  line-height: 28px;
  text-align: center;
  height: 28px;
  width: 100%;
  cursor: pointer;
}
.active {
  color: #fff;
  background-color: #70991f;
  /*   border-bottom: 2px #548ff6 solid; */
}
</style>


