<template>
    <el-row class="home" :gutter="20">
        <el-col :span="8" style="margin-top:20px">
            <el-card shadow="hover">
                <div class="user">
                        <img :src="userImg">
                    <div class="userinfo">
                    <p class="name">Admin</p>
                    <p class="access">超级管理员</p>
                    </div>
                </div>
                <div class="login-info">
                    <p>上次登陆时间：<span>2021-10-28</span></p>
                    <p>上次登陆地点：<span>成都</span></p>
                </div>
            </el-card>
            <el-card style="margin-top:20px;height:430px">
                <el-table :data="tableData">
                    <el-table-column show-overflow-tooltip v-for="(val,key) in tableLabel" :key="key" :prop="key" :label="val"></el-table-column>
                </el-table>
            </el-card>
        </el-col>
        <el-col :span="16" style="margin-top:20px">
            <div class="num">
                <el-card shadow = "hover" v-for="item in countData" :key="item.name" :body-style="{display:'flex',padding:0}">
                    <i class="icon" :class="`el-icon-${item.icon}`" :style="{background:item.color}"></i>
                    <div class="detail">
                        <p class="num">
                            ￥{{item.value}}
                        </p>
                        <p class="txt">
                            ￥{{item.name}}
                        </p>
                    </div>
                </el-card>
            </div>
            <el-card shadow="hover" style="height: 280px">
        <!-- <div style="height: 280px" ref="echart"></div> -->
        <echart :chartData="echartData.order" style="height: 280px"></echart>
      </el-card>
      <div class="graph">
        <el-card shadow="hover" style="height: 260px">
          <echart :chartData="echartData.user" style="height: 240px"></echart>
        </el-card>
        <el-card shadow="hover" style="height: 260px">
          <echart
            :chartData="echartData.video"
            style="height: 240px"
            :isAxisChart="false"
          ></echart>
        </el-card>
      </div>
    </el-col>
  </el-row>
</template>
<script>
import {getHome} from '../../api/data';
import * as echarts from "echarts";
import Echart from "@/components/ECharts.vue";
export default {
   components: {
    Echart,
  },
    data(){
        return{
            userImg:require("../../assets/images/user.png"),
            tableData:[
                {
                    name:"OPPO",
                    todayBuy:1000,
                    totalBuy:30000, 
                    monthBuy:8000,
                },
                 {
                    name:"小米",
                    todayBuy:3200,
                    monthBuy:5000,
                    totalBuy:6090008890,
                },
                 {
                    name:"HUAWEI",
                    todayBuy:1080,
                    monthBuy:5300,
                    totalBuy:70600,
                },
                 {
                    name:"VIVO",
                    todayBuy:7062,
                    monthBuy:70532,
                    totalBuy:42005002,
                },
                 {
                    name:"onePuls",
                    todayBuy:1200,
                    monthBuy:5020,
                    totalBuy:3050501,
                },
                 {
                    name:"苹果",
                    todayBuy:1300,
                    monthBuy:7090,
                    totalBuy:560560,
                },
                 {
                    name:"iqoo",
                    todayBuy:8600,
                    monthBuy:20050,
                    totalBuy:320780,
                },
            ],
            tableLabel:{
                name:'品牌',
                todayBuy:'今日购买',
                monthBuy:'本月购买',
                totalBuy:'总购买',
            },
            countData: [
        {
          name: "今日支付订单",
          value: 1234,
          icon: "success",
          color: "#2ec7c9",
        },
        {
          name: "今日收藏订单",
          value: 210,
          icon: "star-on",
          color: "#ffb980",
        },
        {
          name: "今日未支付订单",
          value: 1234,
          icon: "s-goods",
          color: "#5ab1ef",
        },
        {
          name: "本月支付订单",
          value: 1234,
          icon: "success",
          color: "#2ec7c9",
        },
        {
          name: "本月收藏订单",
          value: 210,
          icon: "star-on",
          color: "#ffb980",
        },
        {
          name: "本月未支付订单",
          value: 1234,
          icon: "s-goods",
          color: "#5ab1ef",
        },
            ],
            echartData: {
        order: {
          xData: [],
          series: [],
        },
        user: {
          xData: [],
          series: [],
        },
        video: {
          series: [],
        },
      },
    };
  },
  methods: {
    getTableData() {
      getHome().then((res) => {
        // console.log(res);
        this.tableData = res.data.tableData;

        // 折线图的展示
        const order = res.data.orderData;
        let keyArray = Object.keys(order.data[0]);

        // const myEchartsOrder = echarts.init(this.$refs.echart);
        // myEchartsOrder.setOption(this.echartsData.order);

        // 传给组件的值
        this.echartData.order.xData = order.date;
        keyArray.forEach((key) => {
          this.echartData.order.series.push({
            name: key,
            data: order.data.map((item) => item[key]),
            type: "line",
          });
        });

        // 用户图

        this.echartData.user.xData = res.data.userData.map((item) => item.date);
        this.echartData.user.series.push({
          name: "新增用户",
          data: res.data.userData.map((item) => item.new),
          type: "bar",
        });
        this.echartData.user.series.push({
          name: "活跃用户",
          data: res.data.userData.map((item) => item.active),
          type: "bar",
        });

        this.echartData.video.series.push({
          data: res.data.videoData,
          type: "pie",
        });
      });
    },
  },
  mounted() {
    this.getTableData();
  },
};
</script>

<style lang="scss" scoped>
    @import"~@/assets/scss/home"
</style>