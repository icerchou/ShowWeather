<template>
  <div id="building" class="outer">

      <!--左侧黑色框-->
      <div class="info_box">
        <!--黑色框内的标题-->
        <h2>雾霾探测系统（自动定位）</h2>
        <!--数据区：包括城市 天气 空气质量-->
        <el-row :gutter="5">
          <el-col :span="12">
          </el-col>
          <el-col :span="12">
            <p class="City">所在城市： {{info.city}}</p>
          </el-col>
        </el-row>

        <el-row :gutter="5">
          <el-col :span="12">
          </el-col>
          <el-col :span="12">
            <p class="Weather">今日天气： {{temperature_today}}℃ {{info.weather}}</p>
          </el-col>
        </el-row>

        <el-row :gutter="5">
          <el-col :span="12">
          </el-col>
          <el-col :span="12">
            <p class="Air">今日空气质量指数： {{info.air}} {{info.air_level}}</p>
          </el-col>
        </el-row>

      </div>
    <!--设置两个折线图的位置和大小-->
    <div id="main0" style="width: 460px;height: 400px;position: absolute;top: 50%;left: 45%;transform: translate(-50%, -50%);"></div>
    <div id="main1" style="width: 460px;height: 400px;position: absolute;top: 50%;left: 80%;transform: translate(-50%, -50%);"></div>
    
  </div>
</template>

<script>
//导入echarts组件 用来绘制折线图
import echarts from 'echarts'
export default {
  name: '',
  data() {
    return {
      charts: '',
      //info：接收获取的天气信息（可拓展）
      info:{
        city:'',//城市名
        weather:'',//天气
        air:'',//空气质量指数
        air_level: ''
      },
      //接受近三天温度
      temperature:[0,0,0],
      temperature_today : 0,
      //接受近三天湿度
      humidity:[0,0,0]
    };
  },
  created(){
    //新建网页时调用getInfo
    this.getInfo();
  },
  methods:{
    //定义异步函数getInfo 获取后端信息
    async getInfo () {
      //获取当天城市名和天气 并赋值
      const { data: res } = await this.$axios.get('https://free-api.heweather.net/s6/weather/now?location=auto_ip&key=f0cff9342d9345ce9d91d6136112680b');
      console.log(res.HeWeather6[0]);
      this.info.city=res.HeWeather6[0].basic.parent_city;
      this.info.weather=res.HeWeather6[0].now.cond_txt;

      //获取当天空气指数 并赋值
      const { data: res0 } = await this.$axios.get('https://free-api.heweather.net/s6/air/now?location=auto_ip&key=f0cff9342d9345ce9d91d6136112680b');
      this.info.air=res0.HeWeather6[0].air_now_city.aqi;
      this.info.air_level=res0.HeWeather6[0].air_now_city.qlty;
      console.log(res0.HeWeather6[0]);

      //获取近3天 温度、湿度
      const { data: res1 } = await this.$axios.get('https://free-api.heweather.net/s6/weather/forecast?location=auto_ip&key=f0cff9342d9345ce9d91d6136112680b');
      console.log(res1.HeWeather6[0]);
      this.temperature[0] = Number(res1.HeWeather6[0].daily_forecast[0].tmp_max);
      this.temperature[1] = Number(res1.HeWeather6[0].daily_forecast[1].tmp_max);
      this.temperature[2] = Number(res1.HeWeather6[0].daily_forecast[2].tmp_max);

      this.humidity[0] = Number(res1.HeWeather6[0].daily_forecast[0].hum);
      this.humidity[1] = Number(res1.HeWeather6[0].daily_forecast[1].hum);
      this.humidity[2] = Number(res1.HeWeather6[0].daily_forecast[2].hum);

      this.temperature_today = this.temperature[0];

      //调用绘制折线图函数：
      this.$nextTick(function() {
                this.drawLine0('main0')
                this.drawLine1('main1')
            })
    },
    //绘制温度变化图
    drawLine0(id) {
                this.charts = echarts.init(document.getElementById(id))
                this.charts.setOption({
                    title:{
                      text:'近三日最高温度变化/℃'
                    },
                    tooltip: {
                        trigger: 'axis'
                    },
                    legend: {
                        data: ['近三日']
                    },
                    grid: {
                        left: '3%',
                        right: '4%',
                        bottom: '3%',
                        containLabel: true
                    },

                    toolbox: {
                        feature: {
                            saveAsImage: {}
                        }
                    },
                    xAxis: {
                        type: 'category',
                        boundaryGap: false,
                    data: ["今天","明天","后天"]
                    
                    },
                    yAxis: {
                        type: 'value'
                    },

                    series: [{
                        name: '最高温度',
                        type: 'line',
                        stack: '总量',
                        data: this.temperature
                    }]
                })
            },
    //绘制湿度变化图
    drawLine1(id) {
                this.charts = echarts.init(document.getElementById(id))
                this.charts.setOption({
                    title:{
                      text:'近三日相对湿度变化/%'
                    },
                    tooltip: {
                        trigger: 'axis'
                    },
                    legend: {
                        data: ['近三日']
                    },
                    grid: {
                        left: '3%',
                        right: '4%',
                        bottom: '3%',
                        containLabel: true
                    },

                    toolbox: {
                        feature: {
                            saveAsImage: {}
                        }
                    },
                    xAxis: {
                        type: 'category',
                        boundaryGap: false,
                    data: ["今天","明天","后天"]
                    
                    },
                    yAxis: {
                        type: 'value'
                    },

                    series: [{
                        name: '相对湿度',
                        type: 'line',
                        stack: '总量',
                        data: this.humidity,
                        color: ['#58afed']
                    }]
                })
            }



  }

};
</script>

<style lang="less" scoped>

  #building{
  background:url("../assets/11.jpg");
  width:100%;			//大小设置为100%
  height:100%;			//大小设置为100%
  position:absolute;
  background-size:100% 100%;}

  .outer {
    height: 100%;
    width: 100%;
  }

  //黑盒子
  .info_box {
    /*让盒子进行位移*/
    position: absolute;
    top: 50%;
    left: 14%;
    transform: translate(-50%, -50%);

    height: 400px;
    width: 400px;
    padding: 40px;
    background: rgba(0, 0, 0, 0.5);
    box-sizing: border-box;
    box-shadow: 0 15px 25px rgba(0, 0, 0, 0.5);
    border-radius: 10px;
  }
  //标题
  .info_box h2 {
    margin: 0 0 30px;
    padding: 0;
    text-align: center;
    color: #fff;
  }

  //城市样式
  .City {
    font-size: x-large;
    font-weight: bolder;
    color: #409eff;
    text-align: center;
    margin-bottom: 50px;
    margin-top: 50px;
  }
  //天气样式
  .Weather {
    font-size: x-large;
    font-weight: bolder;
    color: #d9e626;
    text-align: center;
    margin-bottom: 50px;
    margin-top: 12px;
  }
  //空气指数样式
  .Air {
    font-size: x-large;
    font-weight: bolder;
    color: #ff5d40;
    text-align: center;
    margin-bottom: 50px;
    margin-top: 12px;
  }

</style>