<template>
  <div class="home" style="opacity: .98">
    <div class="head">
      <h1>IRNC-HAPL生产运行监控系统</h1>
      <div class="weather">
        <span id="showTime"></span>
      </div>
    </div>
    <div class="leftChart">
      <div style="height: 18.5rem" class="boxall">
        <div class="allnav" id="chartDay"></div>
        <div class="boxfoot"></div>
      </div>
      <div style="height: 18.5rem" class="boxall">
        <div class="allnav" id="chartMonth"></div>
        <div class="boxfoot"></div>
      </div>
      <div class="test">
        <div style="height: 18.5rem" class="boxall">
          <div class="allnav" id="chartPerTeam"></div>
          <div class="boxfoot"></div>
        </div>
        <div style="height: 18.5rem" class="boxall">
          <div class="allnav" id="chartDayPerLine"></div>
          <div class="boxfoot"></div>
        </div>
        <div style="height: 18.5rem" class="boxall">
          <div class="allnav" id="chartPerLine"></div>
          <div class="boxfoot"></div>
        </div>
      </div>
    </div>
    <div class="midAni">
      <ul class="clearfix">
        <li>
          <div class="bar">
            <div class="barbox">
              <ul class="clearfix">
                <li class="pulll_left counter">856242</li>
                <li class="pulll_left counter">825412</li>
              </ul>
            </div>
            <div class="barbox2">
              <ul class="clearfix">
                <li class="pulll_left">{{currYear}}年总酸洗量</li>
                <li class="pulll_left">{{currYear}}年总出库量</li>
              </ul>
            </div>
          </div>
          <div class="map">
            <div class="map1">
              <img src="../../picture/lbx.png" />
            </div>
            <div class="map2">
              <img src="../../picture/jt.png" />
            </div>
            <div class="map3">
              <img src="../../picture/map.png" />
            </div>
            <!-- <div class="map4" id="map_1"></div> -->
          </div>
        </li>
      </ul>
    </div>
    <div class="rightChart">
      <div class="boxall" style="height:18.5rem">
        <div class="allnav" id="chartLevel1"></div>
        <div class="boxfoot"></div>
      </div>
      <div class="boxall" style="height: 18.5rem">
        <div class="allnav" id="chartMonthBW"></div>
        <div class="boxfoot"></div>
      </div>
      <div class="boxall" style="height: 18.5rem">
        <div class="allnav" id="chartFault"></div>
        <div class="boxfoot"></div>
      </div>
    </div>
    <div class="slo">
      <p :style="{ transform: `rotate(${rotation}deg)` }">{{ slogan }}</p>
    </div>
  </div>
</template>

<script>
import * as echarts from "echarts";
import axios from "axios";
export default {
  data() {
    return {
      currTime: "",
      currYear: 2024,
      slogan: "炼百年不锈  筑万仞青山",
      rotation: 0,
      optMonthLine: {},
      optMonthTeam: {},
      optPerLineDay: {},
      optPerLineMonth: {},
      optPerDayTeam: {},
      optLevel1Month: {},
      optBackMonth: {},
      optFaultMonth: {}
    };
  },
  name: "HelloWorld ,HAPL",
  props: {
    msg: String
  },
  mounted() {
    this.fetchData();
    this.updateDay();
    this.updateMonth();
    this.updatePerTeam();
    this.updatePerLine();
    this.updateDayPerLine();
    this.updateTime();
    this.updateMonthLevel1();
    this.updateMonthBW();
    this.updateMonthFault();
  },
  methods: {
    fetchData() {
      axios
        .get("http://jsonplaceholder.typicode.com/posts")
        .then(response => {
          console.log(response);
        })
        .catch(err => {
          console.log(err);
        });
    },
    updateMonthFault() {
      var myChart = echarts.init(document.getElementById("chartFault"));

      var option = {
        title: [
          {
            text: "本月故障率",
            left: "center",
            textStyle: {
              color: "#fff",
              fontSize: "16"
            }
          }
        ],
        tooltip: {
          trigger: "axis",
          axisPointer: {
            lineStyle: {
              color: "#dddc6b"
            }
          }
        },

        grid: {
          left: "10",
          top: "30",
          right: "10",
          bottom: "10",
          containLabel: true
        },

        xAxis: [
          {
            type: "category",
            boundaryGap: false,
            axisLabel: {
              textStyle: {
                color: "rgba(255,255,255,.6)",
                fontSize: 12
              }
            },
            axisLine: {
              lineStyle: {
                color: "rgba(255,255,255,.2)"
              }
            },
            data: [
              "01",
              "02",
              "03",
              "04",
              "05",
              "06",
              "07",
              "08",
              "09",
              "11",
              "12",
              "13",
              "14",
              "15",
              "16",
              "17",
              "18",
              "19",
              "20",
              "21",
              "22",
              "23",
              "24",
              "25",
              "26",
              "27",
              "28",
              "29",
              "30"
            ]
          },
          {
            axisPointer: {
              show: false
            },
            axisLine: {
              show: false
            },
            position: "bottom",
            offset: 20
          }
        ],

        yAxis: [
          {
            type: "value",
            axisTick: {
              show: false
            },
            axisLine: {
              lineStyle: {
                color: "rgba(255,255,255,.1)"
              }
            },
            axisLabel: {
              formatter: function(value) {
                return value.toFixed(1);
              },
              textStyle: {
                color: "rgba(255,255,255,.6)",
                fontSize: 12
              }
            },
            splitLine: {
              lineStyle: {
                color: "rgba(255,255,255,.1)"
              }
            }
          }
        ],
        series: [
          {
            max: 3,
            min: 0,
            name: "故障率",
            type: "line",
            smooth: false,
            symbol: "circle",
            symbolSize: 5,
            showSymbol: false,
            markPoint: {
              data: [
                {
                  type: "max",
                  name: "最大值"
                },
                {
                  type: "min",
                  name: "最小值"
                }
              ]
            },
            markLine: {
              symbol: "none",
              label: {
                show: true,
                position: "start",
                formatter: function(value) {
                  return parseFloat(value.value).toFixed(2);
                }
              },
              data: [
                {
                  type: "average",
                  name: "平均值"
                }
              ]
            },
            lineStyle: {
              normal: {
                color: "#ffaa00",
                width: 2
              }
            },
            areaStyle: {
              normal: {
                color: new echarts.graphic.LinearGradient(
                  0,
                  0,
                  0,
                  1,
                  [
                    {
                      offset: 0,
                      color: "rgba(0, 216, 135, 0.4)"
                    },
                    {
                      offset: 0.8,
                      color: "rgba(0, 216, 135, 0.1)"
                    }
                  ],
                  false
                ),
                shadowColor: "rgba(0, 0, 0, 0.1)"
              }
            },
            itemStyle: {
              normal: {
                color: "#00d887",
                borderColor: "rgba(221, 220, 107, .1)",
                borderWidth: 12
              }
            },
            data: [
              0.0,
              0.12,
              0.0,
              0.02,
              0.87,
              0.0,
              0.11,
              0.0,
              0.0,
              0.0,
              0.95,
              0.0,
              0.0,
              0.35,
              0.0,
              1.28,
              0.55,
              0.0,
              0.09,
              0.12,
              0.32,
              0.0,
              0.0,
              0.14,
              0.0,
              0.0,
              0.0,
              2.15,
              0.24,
              0.0
            ]
          }
        ]
      };

      // 使用刚指定的配置项和数据显示图表。
      myChart.setOption(option);
      window.addEventListener("resize", function() {
        myChart.resize();
      });
    },
    updateMonthBW() {
      var myChart = echarts.init(document.getElementById("chartMonthBW"));
      // 绘制图表
      myChart.setOption({
        title: [
          {
            text: "本月返洗率",
            left: "center",
            textStyle: {
              color: "#fff",
              fontSize: "16"
            }
          }
        ],
        tooltip: {
          trigger: "axis",
          axisPointer: {
            lineStyle: {
              color: "#dddc6b"
            }
          }
        },
        legend: {
          top: "0%",
          data: ["返洗率"],
          textStyle: {
            color: "rgba(255,255,255,.5)",
            fontSize: "12"
          }
        },
        grid: {
          left: "10",
          top: "30",
          right: "10",
          bottom: "10",
          containLabel: true
        },

        xAxis: [
          {
            type: "category",
            boundaryGap: false,
            axisLabel: {
              textStyle: {
                color: "rgba(255,255,255,.6)",
                fontSize: 12
              }
            },
            axisLine: {
              lineStyle: {
                color: "rgba(255,255,255,.2)"
              }
            },

            data: [
              "01",
              "02",
              "03",
              "04",
              "05",
              "06",
              "07",
              "08",
              "09",
              "11",
              "12",
              "13",
              "14",
              "15",
              "16",
              "17",
              "18",
              "19",
              "20",
              "21",
              "22",
              "23",
              "24",
              "25",
              "26",
              "27",
              "28",
              "29",
              "30"
            ]
          },
          {
            axisPointer: {
              show: false
            },
            axisLine: {
              show: false
            },
            position: "bottom",
            offset: 20
          }
        ],

        yAxis: [
          {
            type: "value",
            axisTick: {
              show: false
            },
            axisLine: {
              lineStyle: {
                color: "rgba(255,255,255,.1)"
              }
            },
            axisLabel: {
              formatter: function(value) {
                return value.toFixed(1);
              },
              textStyle: {
                color: "rgba(255,255,255,.6)",
                fontSize: 12
              }
            },

            splitLine: {
              lineStyle: {
                color: "rgba(255,255,255,.1)"
              }
            }
          }
        ],
        series: [
          {
            name: "返洗率",
            type: "line",
            smooth: false,
            symbol: "circle",
            symbolSize: 5,
            showSymbol: false,
            markPoint: {
              data: [
                {
                  type: "max",
                  name: "最大值"
                },
                {
                  type: "min",
                  name: "最小值"
                }
              ]
            },
            markLine: {
              symbol: "none",
              label: {
                show: true,
                position: "start",
                formatter: function(value) {
                  return parseFloat(value.value).toFixed(2);
                }
              },
              data: [
                {
                  type: "average",
                  name: "平均值"
                }
              ]
            },
            lineStyle: {
              normal: {
                color: "#00d887",
                width: 2
              }
            },
            areaStyle: {
              normal: {
                color: new echarts.graphic.LinearGradient(
                  0,
                  0,
                  0,
                  1,
                  [
                    {
                      offset: 0,
                      color: "rgba(0, 216, 135, 0.4)"
                    },
                    {
                      offset: 0.8,
                      color: "rgba(0, 216, 135, 0.1)"
                    }
                  ],
                  false
                ),
                shadowColor: "rgba(0, 0, 0, 0.1)"
              }
            },
            itemStyle: {
              normal: {
                color: "#00d887",
                borderColor: "rgba(221, 220, 107, .1)",
                borderWidth: 12
              }
            },
            data: [
              0.52,
              0.72,
              0.2,
              0.12,
              0.0,
              0.72,
              0.46,
              0.73,
              0.52,
              0.23,
              0.37,
              0.96,
              0.52,
              0.24,
              0.76,
              0.52,
              0.55,
              0.42,
              0.09,
              0.12,
              0.32,
              0.62,
              0.92,
              0.14,
              0.44,
              0.64,
              0.0,
              0.44,
              0.24,
              0.25
            ]
          }
        ]
      });
    },
    updateMonthLevel1() {
      var myChart = echarts.init(document.getElementById("chartLevel1"));
      // 绘制图表
      myChart.setOption({
        title: [
          {
            text: "本月一级品率",
            left: "center",
            textStyle: {
              color: "#fff",
              fontSize: "16"
            }
          }
        ],
        tooltip: {
          trigger: "axis",
          axisPointer: {
            lineStyle: {
              color: "#dddc6b"
            }
          }
        },
        legend: {
          top: "0%",
          data: ["一级品率"],
          textStyle: {
            color: "rgba(255,255,255,.5)",
            fontSize: "12"
          }
        },
        grid: {
          left: "10",
          top: "30",
          right: "10",
          bottom: "10",
          containLabel: true
        },

        xAxis: [
          {
            type: "category",
            boundaryGap: false,
            axisLabel: {
              textStyle: {
                color: "rgba(255,255,255,.6)",
                fontSize: 12
              }
            },
            axisLine: {
              lineStyle: {
                color: "rgba(255,255,255,.2)"
              }
            },

            data: [
              "01",
              "02",
              "03",
              "04",
              "05",
              "06",
              "07",
              "08",
              "09",
              "11",
              "12",
              "13",
              "14",
              "15",
              "16",
              "17",
              "18",
              "19",
              "20",
              "21",
              "22",
              "23",
              "24",
              "25",
              "26",
              "27",
              "28",
              "29",
              "30"
            ]
          },
          {
            axisPointer: {
              show: false
            },
            axisLine: {
              show: false
            },
            position: "bottom",
            offset: 20
          }
        ],

        yAxis: [
          {
            max: 100,
            min: 98.5,
            type: "value",
            axisTick: {
              show: false
            },
            axisLine: {
              lineStyle: {
                color: "rgba(255,255,255,.1)"
              }
            },
            axisLabel: {
              textStyle: {
                color: "rgba(255,255,255,.6)",
                fontSize: 12
              }
            },

            splitLine: {
              lineStyle: {
                color: "rgba(255,255,255,.1)"
              }
            }
          }
        ],
        series: [
          {
            name: "一级品率",
            type: "line",
            smooth: false,
            symbol: "circle",
            symbolSize: 10,
            showSymbol: false,

            markPoint: {
              data: [
                {
                  type: "max",
                  name: "最大值"
                },
                {
                  type: "min",
                  name: "最小值"
                }
              ]
            },
            markLine: {
              symbol: "none",
              label: {
                show: true,
                position: "start",
                formatter: function(value) {
                  return parseFloat(value.value).toFixed(2);
                }
              },
              data: [
                {
                  type: "average",
                  name: "平均值"
                }
              ]
            },
            lineStyle: {
              normal: {
                color: "#0184d5",
                width: 2
              }
            },
            areaStyle: {
              normal: {
                color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                  {
                    offset: 0,
                    color: "rgba(1, 132, 213, 0.4)"
                  },
                  {
                    offset: 0.8,
                    color: "rgba(1, 132, 213, 0.1)"
                  }
                ]),
                shadowColor: "rgba(0, 0, 0, 0.1)"
              }
            },
            itemStyle: {
              normal: {
                color: "#0184d5",
                borderColor: "rgba(221, 220, 107, .1)",
                borderWidth: 12
              }
            },

            data: [
              99.91,
              99.72,
              99.55,
              99.85,
              99.75,
              99.85,
              99.42,
              99.02,
              99.71,
              100,
              99.61,
              99.71,
              99.41,
              99.31,
              99.71,
              99.81,
              99.91,
              99.61,
              99.81,
              99.81,
              99.61,
              99.72,
              99.67,
              99.88,
              99.95,
              99.62,
              99.77,
              99.95,
              99.21,
              100
            ]
          }
        ]
      });
    },
    updateDayPerLine() {
      // 基于准备好的dom，初始化echarts实例
      var myChart = echarts.init(document.getElementById("chartDayPerLine"));
      // 绘制图表
      myChart.setOption({
        title: [
          {
            text: "本日各线占比",
            left: "center",
            textStyle: {
              color: "#fff",
              fontSize: "16"
            }
          }
        ],
        tooltip: {
          trigger: "item",
          formatter: "{a} <br/>{b}: {c} ({d}%)",
          position: function(p) {
            //其中p为当前鼠标的位置
            return [p[0] + 10, p[1] - 10];
          }
        },
        legend: {
          top: "70%",
          itemWidth: 10,
          itemHeight: 10,
          data: ["1#", "2#", "3#", "4#"],
          textStyle: {
            color: "rgba(255,255,255,.5)",
            fontSize: "12"
          }
        },
        series: [
          {
            name: "占比",
            type: "pie",
            center: ["50%", "42%"],
            radius: ["40%", "60%"],
            color: [
              "#ffaa00",
              "#066eab",
              "#0055ff",
              "#ffffff",
              "#06a0ab",
              "#00aaff",
              "#aa00ff",
              "#aa0000",
              "#06f0ab"
            ],
            label: {
              show: false,
              normal: {
                show: true,
                position: "inner",
                textstyle: {
                  fontsize: 12,
                  color: "white",
                  fontWeight: 100
                },
                formatter: "{d}%"
              }
            },
            labelLine: {
              show: false
            },
            data: [
              {
                value: 32152,
                name: "1#"
              },
              {
                value: 0,
                name: "2#"
              },
              {
                value: 35868,
                name: "3#"
              },
              {
                value: 54215,
                name: "4#"
              }
            ]
          }
        ]
      });
    },
    updatePerLine() {
      // 基于准备好的dom，初始化echarts实例
      var myChart = echarts.init(document.getElementById("chartPerLine"));
      // 绘制图表
      myChart.setOption({
        title: [
          {
            text: "本月各线占比",
            left: "center",
            textStyle: {
              color: "#fff",
              fontSize: "16"
            }
          }
        ],
        tooltip: {
          trigger: "item",
          formatter: "{a} <br/>{b}: {c} ({d}%)",
          position: function(p) {
            //其中p为当前鼠标的位置
            return [p[0] + 10, p[1] - 10];
          }
        },
        legend: {
          top: "70%",
          itemWidth: 10,
          itemHeight: 10,
          data: ["1#", "2#", "3#", "4#"],
          textStyle: {
            color: "rgba(255,255,255,.5)",
            fontSize: "12"
          }
        },
        series: [
          {
            name: "本月各线占比",
            type: "pie",
            center: ["50%", "42%"],
            radius: ["40%", "60%"],
            color: [
              "#ffaa00",
              "#066eab",
              "#0055ff",
              "#ffffff",
              "#06a0ab",
              "#00aaff",
              "#aa00ff",
              "#aa0000",
              "#06f0ab"
            ],
            label: {
              show: false,
              normal: {
                show: true,
                position: "inner",
                textstyle: {
                  fontsize: 12,
                  color: "white",
                  fontWeight: 100
                },
                formatter: "{d}%"
              }
            },
            labelLine: {
              show: false
            },
            data: [
              {
                value: 5,
                name: "1#"
              },
              {
                value: 1,
                name: "2#"
              },
              {
                value: 6,
                name: "3#"
              },
              {
                value: 2,
                name: "4#"
              }
            ]
          }
        ]
      });
    },
    updatePerTeam() {
      // 基于准备好的dom，初始化echarts实例
      var myChart = echarts.init(document.getElementById("chartPerTeam"));
      // 绘制图表
      myChart.setOption({
        title: [
          {
            text: "本月各班占比",
            left: "center",
            textStyle: {
              color: "#fff",
              fontSize: "16"
            }
          }
        ],
        tooltip: {
          trigger: "item",
          formatter: "{a} <br/>{b}: {c} ({d}%)",
          position: function(p) {
            //其中p为当前鼠标的位置
            return [p[0] + 10, p[1] - 10];
          }
        },
        legend: {
          top: "70%",
          itemWidth: 10,
          itemHeight: 10,
          data: ["A", "B", "C"],
          textStyle: {
            color: "rgba(255,255,255,.5)",
            fontSize: "12"
          }
        },
        series: [
          {
            name: "本月各班占比",
            type: "pie",
            center: ["50%", "42%"],
            radius: ["40%", "60%"],
            color: [
              "#ffaa00",
              "#066eab",
              "#0055ff",
              "#ffffff",
              "#06a0ab",
              "#00aaff",
              "#aa00ff",
              "#aa0000",
              "#06f0ab"
            ],
            label: {
              show: false,
              normal: {
                show: true,
                position: "inner",
                textstyle: {
                  fontsize: 12,
                  color: "white",
                  fontWeight: 100
                },
                formatter: "{d}%"
              }
            },
            labelLine: {
              show: true
            },
            data: [
              {
                value: 42152,
                name: "A"
              },
              {
                value: 41254,
                name: "B"
              },
              {
                value: 39875,
                name: "C"
              }
            ]
          }
        ]
      });
    },
    updateTime() {
      var t = null;
      t = setTimeout(time, 1000); //開始运行
      function time() {
        clearTimeout(t); //清除定时器
        let dt = new Date();
        var y = dt.getFullYear();
        this.currYear = y;
        var mt = dt.getMonth() + 1;
        var day = dt.getDate();
        var h = dt.getHours(); //获取时
        var m = dt.getMinutes(); //获取分
        var s = dt.getSeconds(); //获取秒
        document.getElementById("showTime").innerHTML =
          y + "年" + mt + "月" + day + "日  " + h + "时" + m + "分" + s + "秒";
        t = setTimeout(time, 1000); //设定定时器，循环运行
      }
    },
    updateDay() {
      // 基于准备好的dom，初始化echarts实例
      var myChart = echarts.init(document.getElementById("chartDay"));
      // 绘制图表
      myChart.setOption({
        title: [
          {
            text: "本日各线产量",
            left: "center",
            textStyle: {
              color: "#fff",
              fontSize: "16"
            }
          }
        ],
        tooltip: {
          trigger: "axis",
          axisPointer: {
            type: "shadow"
          }
        },
        grid: {
          left: "0%",
          top: "10px",
          right: "0%",
          bottom: "4%",
          containLabel: true
        },
        xAxis: [
          {
            type: "category",
            data: ["1#", "2#", "3#", "4#"],
            axisLine: {
              show: true,
              lineStyle: {
                color: "rgba(255,255,255,.1)",
                width: 1,
                type: "solid"
              }
            },

            axisTick: {
              show: false
            },
            axisLabel: {
              interval: 0,
              // rotate:50,
              show: true,
              splitNumber: 15,
              textStyle: {
                color: "rgba(255,255,255,.6)",
                fontSize: "12"
              }
            }
          }
        ],
        yAxis: [
          {
            type: "value",
            axisLabel: {
              // formatter: '{value}%'
              show: true,
              textStyle: {
                color: "rgba(255,255,255,.6)",
                fontSize: "12"
              }
            },
            axisTick: {
              show: false
            },
            axisLine: {
              show: true,
              lineStyle: {
                color: "rgba(255,255,255,.1	)",
                width: 1,
                type: "solid"
              }
            },
            splitLine: {
              lineStyle: {
                color: "rgba(255,255,255,.1)"
              }
            }
          }
        ],
        series: [
          {
            type: "bar",
            data: [524, 0, 1258, 1852],
            barWidth: "45%", //柱子宽度
            // barGap: 1, //柱子之间间距
            itemStyle: {
              normal: {
                label: {
                  show: true,
                  position: "top",
                  textstyle: {
                    color: "black",
                    fontsize: 16
                  }
                },
                color: "#2f89cf",
                opacity: 1,
                barBorderRadius: 5
              }
            }
          }
        ]
      });
    },
    updateMonth() {
      // 基于准备好的dom，初始化echarts实例
      var myChart = echarts.init(document.getElementById("chartMonth"));
      // 绘制图表
      myChart.setOption({
        title: [
          {
            text: "本月各线产量",
            left: "center",
            textStyle: {
              color: "#fff",
              fontSize: "16"
            }
          }
        ],
        tooltip: {
          trigger: "axis",
          axisPointer: {
            type: "shadow"
          }
        },
        grid: {
          left: "0%",
          top: "10px",
          right: "0%",
          bottom: "4%",
          containLabel: true
        },
        xAxis: [
          {
            type: "category",
            data: ["1#", "2#", "3#", "4#"],
            axisLine: {
              show: true,
              lineStyle: {
                color: "rgba(255,255,255,.1)",
                width: 1,
                type: "solid"
              }
            },

            axisTick: {
              show: false
            },
            axisLabel: {
              interval: 0,
              // rotate:50,
              show: true,
              splitNumber: 15,
              textStyle: {
                color: "rgba(255,255,255,.6)",
                fontSize: "12"
              }
            }
          }
        ],
        yAxis: [
          {
            type: "value",
            axisLabel: {
              //formatter: '{value} %'
              show: true,
              textStyle: {
                color: "rgba(255,255,255,.6)",
                fontSize: "12"
              }
            },
            axisTick: {
              show: false
            },
            axisLine: {
              show: true,
              lineStyle: {
                color: "rgba(255,255,255,.1	)",
                width: 1,
                type: "solid"
              }
            },
            splitLine: {
              lineStyle: {
                color: "rgba(255,255,255,.1)"
              }
            }
          }
        ],
        series: [
          {
            type: "bar",
            data: [32152, 15845, 35868, 54215],
            barWidth: "45%", //柱子宽度
            // barGap: 1, //柱子之间间距
            itemStyle: {
              normal: {
                label: {
                  show: true,
                  position: "top",
                  textstyle: {
                    color: "black",
                    fontsize: 16
                  }
                },
                color: "#27d08a",
                opacity: 1,
                barBorderRadius: 5
              }
            }
          }
        ]
      });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.slo {
  margin-top: 30%;
  display: flex;
  justify-content: center; /* 水平居中 */
  align-items: center; /* 垂直居中 */
  height: 30%;
}
p {
  bottom: 10%;
  font-size: 36px;
  padding: 10px;
  display: inline-block;
  color: chartreuse;
}
.boxall4 {
  border: 1px solid rgba(25, 186, 139, 0.17);
  padding: 0.6rem 0.9rem 0.6rem 0.9rem;
  background: rgba(255, 255, 255, 0.04) url(../../images/line.png);
  background-size: 100% auto;
  position: relative;
  margin-bottom: 0.9rem;
  z-index: 10;
  width: 90%;
  height: 33%;
}
.boxall4:before,
.boxall4:after {
  position: relative;
  width: 0.5rem;
  height: 0.5rem;
  content: "";
  border-top: 2px solid #02a6b5;
  top: 0;
}
.boxall4.allnav,
.boxfoot4:before {
  border-left: 2px solid #02a6b5;
  left: 0;
}
.boxall4:after,
.boxfoot4:after {
  border-right: 2px solid #02a6b5;
  right: 0;
}
.rightChart {
  position: absolute;
  top: 10%;
  width: 30%;
  height: 90%;
  margin-left: 69%;
  display: flex;
  flex-direction: column;
}
.dayMonthSum {
  width: 100%;
  height: 5rem;

  display: flex;
  flex-direction: column;
}
.test {
  display: flex;
  flex-direction: row;
  width: 100%;
  height: 30%;
}
.boxall3 {
  border: 1px solid rgba(25, 186, 139, 0.17);
  padding: 0.6rem 0.9rem 0.6rem 0.9rem;
  background: rgba(255, 255, 255, 0.04) url(../../images/line.png);
  background-size: 100% auto;
  position: relative;
  margin-bottom: 0.9rem;
  z-index: 10;
  width: 100%;
  height: 33%;
}
.boxall3:before,
.boxall3:after {
  position: relative;
  width: 0.5rem;
  height: 0.5rem;
  content: "";
  border-top: 2px solid #02a6b5;
  top: 0;
}
.boxall3:before,
.boxfoot3:before {
  border-left: 2px solid #02a6b5;
  left: 0;
}
.boxall3:after,
.boxfoot3:after {
  border-right: 2px solid #02a6b5;
  right: 0;
}
.alltitle {
  font-size: 2rem;
  font-weight: 300;
  color: #fff;
  text-align: center;
  line-height: 0.5rem;
}

.weather {
  position: absolute;
  right: 1rem;
  top: 1.5rem;
  line-height: 0.75rem;
}
.weather span {
  color: rgba(255, 255, 255, 0.7);
  font-size: 1.18rem;
  padding-right: 0.5rem;
  margin-top: 0.5rem;
}
.allnav1 {
  height: 100%;
  margin-top: 0.7rem;
}
.allnav {
  height: calc(100% - 5px);
  margin-top: 0.7rem;
}
.head h1 {
  color: #fff;
  text-align: center;
  vertical-align: center;
  font-size: 2.1rem;
  line-height: 2.4rem;
  margin-top: 0%;
  height: 4rem;
  position: relative;
  padding-top: 1rem;
}
.leftChart {
  position: absolute;
  top: 10%;
  width: 30%;
  height: 90%;
  margin-left: 1%;
  display: flex;
  flex-direction: column;
}
.midAni {
  position: absolute;
  top: 10%;
  width: 36%;
  height: 90%;
  margin-left: 32%;
  display: flex;
  flex-direction: column;
}
.home {
  position: absolute;
  width: 100%;
  left: 0;
  top: 0;
  height: 100%;
  z-index: 10;
  background: url(../../images/bg.jpg) no-repeat center center;
}
.head {
  height: 9%;
  width: 100%;
  background: url(../../images/head_bg.png) no-repeat center center;
  background-size: 100% 100%;
  position: relative;
  z-index: 100;
}
.boxall {
  border: 1px solid rgba(25, 186, 139, 0.17);
  padding: 0.6rem 0.9rem 0.6rem 0.9rem;
  background: rgba(255, 255, 255, 0.04) url(../../images/line.png);
  background-size: 100% auto;
  position: relative;
  margin-bottom: 0.9rem;
  z-index: 10;
  width: 100%;
  height: 33%;
}
.boxall:before,
.boxall:after {
  position: absolute;
  width: 0.5rem;
  height: 0.5rem;
  content: "";
  border-top: 2px solid #02a6b5;
  top: 0;
}
.boxall:before,
.boxfoot:before {
  border-left: 2px solid #02a6b5;
  left: 0;
}
.boxall:after,
.boxfoot:after {
  border-right: 2px solid #02a6b5;
  right: 0;
}
.alltitle {
  font-size: 0.2rem;
  color: #fff;
  text-align: center;
  line-height: 0.5rem;
}

.boxfoot {
  position: absolute;
  bottom: 0;
  width: 100%;
  left: 0;
}
.boxfoot:before,
.boxfoot:after {
  position: absolute;
  width: 0.5rem;
  height: 0.5rem;
  content: "";
  border-bottom: 2px solid #02a6b5;
  bottom: 0;
}
</style>
