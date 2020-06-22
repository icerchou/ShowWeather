<template>
  <div class="outer">

    <div class="block">

      <!--用来设置悬浮黑色框-->
      <div class="info_box">
        <!--标题-->
        <h2>雾霾探测系统（自动定位）</h2>
        <!--表单区域-->

        <el-row :gutter="5">
          <el-col :span="12">
          </el-col>
          <el-col :span="12">
            <p class="count-fine">城市： {{city}}</p>
          </el-col>
        </el-row>

        <el-row :gutter="5">
          <el-col :span="12">
          </el-col>
          <el-col :span="12">
            <p class="count-returnPeriod">天气： {{weather}}</p>
          </el-col>
        </el-row>

        <el-row :gutter="5">
          <el-col :span="12">
          </el-col>
          <el-col :span="12">
            <p class="count-depoist">空气质量： {{air}}</p>
          </el-col>
        </el-row>

      </div>

    </div>
    
  </div>
</template>

<script>
export default {
  data() {
    return {
      Key:{
        appid:'67293484',
        appsecret:'eieSox8k',
        version:'v6'
      },
      info:{
        cityid:'',
        city:'',
        weather:'',
        air:''
      }
    };
  },
  created(){
      this.getInfo();
  },
  methods:{
    async getInfo () {
      this.axios.get('https://free-api.heweather.net/s6/weather/now?location=beijing&key=f0cff9342d9345ce9d91d6136112680b')
    }
  }

};
</script>

<style lang="less" scoped>
.outer {
  height: 100%;
  width: 100%;
}


//带图片区域
.block {
  background-color: #2b4b6b;
  /* background-size: 100% 100%; */
  background-size: cover;
  /* background-repeat: no-repeat; */
  height: 100%;
  width: 100%;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
}

//黑盒子
.info_box {
  /*让盒子进行位移*/
  // position: absolute;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);

  height: 500px;
  width: 400px;
  padding: 40px;
  background: rgba(0, 0, 0, 0.8);
  box-sizing: border-box;
  box-shadow: 0 15px 25px rgba(0, 0, 0, 0.5);
  border-radius: 10px;
}

.info_box h2 {
  //这个margin再说吧
  margin: 0 0 30px;
  padding: 0;
  text-align: center;
  color: #fff;
}

.el-tag {
  height: 40px;
  width: 100px;
  font-size: larger;
  font-weight: bold;
  // text-align: center;
  // align-content: center;
  // align-items: center;
  margin-bottom: 15px;
  margin-top: 12px;
}

.el-row {
  margin-bottom: 20px;
  &:last-child {
    margin-bottom: 0;
  }
}
.el-col {
  border-radius: 4px;
}

.count-fine {
  font-size: xx-large;
  font-weight: bolder;
  color: #409eff;
  text-align: center;
  margin-bottom: 15px;
  margin-top: 12px;
}

.count-depoist {
  font-size: xx-large;
  font-weight: bolder;
  color: #d9e626;
  text-align: center;
  margin-bottom: 15px;
  margin-top: 12px;
}

.count-returnPeriod {
  font-size: xx-large;
  font-weight: bolder;
  color: #ff5d40;
  text-align: center;
  margin-bottom: 15px;
  margin-top: 12px;
}

.bg-purple-dark {
  background: #99a9bf;
}
.bg-purple {
  background: #d3dce6;
}
.bg-purple-light {
  background: #e5e9f2;
}
.grid-content {
  border-radius: 4px;
  min-height: 36px;
}
.row-bg {
  padding: 10px 0;
  background-color: #f9fafc;
}
</style>