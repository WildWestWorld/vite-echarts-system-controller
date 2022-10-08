<template>
  <!-- Echart 默认宽高是0 -->
  <div ref="myChart" class="myEchartStyle">Vite</div>
  <div ref="pieChart" class="myEchartStyle">Pie</div>
  <div ref="lineChart" class="myEchartStyle">Line</div>
  <div ref="scatterChart" class="myEchartStyle">Scatter</div>
  <div ref="KLineChart" class="myEchartStyle">KLineChart</div>
  <div ref="radarChart" class="myEchartStyle">radarChart</div>
  <div ref="funnelChart" class="myEchartStyle">funnelChart</div>
  <div ref="gaugeChart" class="myEchartStyle">deviceChart</div>
  <div ref="linkChart" class="myEchartStyle">linkChart</div>
  <div ref="treeChart" class="myEchartStyle">treeChart</div>
  <div ref="sortChart" class="myEchartStyle">SortChart</div>
  <div ref="chinaMap" class="myEchartStyle">chinaMap</div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import * as echarts from 'echarts';
import { computed } from '@vue/reactivity';
import { roma } from '../assets/index';

//引用axios
import axios from 'axios';

// 常量区
const myChart = ref(null);
const pieChart = ref(null);
const lineChart = ref(null);
const scatterChart = ref(null);
const KLineChart = ref(null);
const radarChart = ref(null);
const funnelChart = ref(null);
const gaugeChart = ref(null);
const linkChart = ref(null);
const treeChart = ref(null);
const sortChart = ref(null);
const chinaMap = ref(null);

const KlineData = ref([
  [20, 34, 10, 38],
  [40, 35, 30, 50],
  [20, 34, 10, 38],
]);
//  让K线图中出现折线图，所以我们把放到第一个去
const newKlineData = computed(() => {
  return KlineData.value.map((item) => {
    return item[0];
  });
});

//生命周期区
onMounted(() => {
  linkDataAPI();

  Test();
  //折线图
  let myEcharts = echarts.init(myChart.value);


//   点击事件
  myEcharts.on('click', (params) => {
    console.log(params);
    console.log('事件');
  });
  //只针对你页面中的折线图("series.line")进行点击事件监听
  myEcharts.on('click',"series.line",(params)=>{
    console.log(params)
  }) 
//   只针对你页面中某个图的某个属性 进行点击事件进行监听
//seriesIndex:1 =>1 = 折现图的索引 也可以代表 折线图
  myEcharts.on('click',{name:"数码",seriesIndex:1},(params)=>{
    console.log(params)
  })
  //设置参数
  myEcharts.setOption({
    //关闭/开启动画
    animation: false,
    //动画数量的阈值，若是echats中的动画数量超过阈值，他就不会生效
    animationThreshold: 4,
    //设置动画的时长
    animationDuration: 5000,
    //动画的延迟时间
    animationDelay: 1000,
    // 标题
    title: {
      text: 'Echart',
      //   点击标题跳转
      link: 'http://www.baidu.com',
      //   target: 'self',当前页面跳转 不设置就是打开另外一个页面
      target: 'self',
      //   标题的文本框的颜色
      backgroundColor: 'white',
      //   设置border颜色
      borderColor: 'skyblue',
      //   标题的border的宽度
      borderWidth: 5,

      //页面标题文本居中
      x: 'left',
      //   主标题文字的样式
      textStyle: {
        color: 'blue',
        fontSize: 20,
      },
      //   副标题
      subtext: 'helloEcharts',
      //   副标题的超链接
      sublink: 'http://www.baidu.com',
      //   副标题的文字样式
      subtextStyle: {
        color: 'grey',
        fontSize: 12,
      },
    },
    //网格
    grid: {
      //给整个图标加个边框
      show: true,
      //left,top...会让图标与最外面大容器的距离远离
      left: '20%',
      top: '20%',
      //整个图表背景的颜色
      backgroundColor: 'pink',
      //整个图表的边框颜色
      borderColor: 'red',
    },
    // 鼠标移动到柱状图时显示的一个提示框
    tooltip: {
      // 设置触发类型/触发范围 axis/item
      //   axis在图形以外的地方也能触发
      //item在图形内才会触发
      trigger: 'axis',

      //  坐标轴指示器/鼠标位置指示器，也就是鼠标点击上去后 变化
      axisPointer: {
        type: 'cross', // line显示一个实线 shadow 阴影效果 cross 十字准心
      },
      //   是否显示悬浮框
      showContent: true,
      // 以下 悬浮框的样式
      //  背景颜色
      backgroundColor: 'white',
      //border的颜色
      borderColor: 'blue',
      //   文字的样式
      textStyle: {
        color: 'grey',
        fontSize: 10,
      },
      //   自定义提示框信息
      //   函数必须取名叫formatter才有效，取其他的名字是无效的
      formatter(params) {
        // console.log(params);
        for (var i = 0; i < params.length; i++) {
          return '名字' + params[i].data.value + '--时间' + params[i].data.date;
        }
      },
    },
    // 图例
    legend: {
      //设置图例的开启或是关闭
      show: true,
      //图例的图标的样式
      icon: 'circle',
      //图例的宽
      itemWidth: 10,
      //图例的高度
      itemHeight: 20,
      //   图例的文字样式
      textStyle: {
        color: 'red',
        fontSize: 30,
      },
    },
    //横坐标内容
    xAxis: {
      type: 'value', //坐标轴的类型，value数据轴，category 类目轴
    },
    //纵坐标内容
    yAxis: {
      data: ['EZ', 'VN', 'Victor'],
      type: 'category', //坐标轴的类型，value数据轴，category 类目轴
    },
    //系列
    series: {
      name: 'LOL英雄大全',
      type: 'bar',
      data: [
        { value: '5', date: '2020-1-1' },
        { value: '5', date: '2030-1-1' },
        { value: '25', date: '2040-1-1' },
      ],
      //   设置柱状图的宽度
      barWidth: 20,
      //   主题色
      color: 'red',
      //单独设置某一条柱状图的颜色
      itemStyle: {
        normal: {
          color: function (params) {
            let colorList = ['#c23531', '#2f4554', '#61a0a8', '#91c7ae'];
            // 利用params 里面的dataIndex属性我们就能让对应的柱状图设置需要的颜色
            console.log(params);
            return colorList[params.dataIndex];
          },
        },
      },
      //   最大值/最小值
      markPoint: {
        data: [
          { type: 'max', name: '最大值' },
          { type: 'min', name: '最小值' },
        ],
      },
      //  图表中的标线
      markLine: {
        data: [
          {
            type: 'average', //平均值
            name: '平均值',
          },
        ],
      },
    },
    //区域缩放
    dataZoom: [
      {
        //条形来控制缩放
        type: 'slider',
        xAxisIndex: 0, //设置x轴最开始缩放的位置
        fliterMode: 'none', //过滤器 暂不知道 有何作用
      },
      {
        //条形来控制缩放
        type: 'slider',
        yAxisIndex: 0, //设置y轴最开始缩放的位置
        fliterMode: 'none', //过滤器 暂不知道 有何作用
      },
    ],
  });
  //  圆图
  let myPieEcharts = echarts.init(pieChart.value);

  let pieData = [
    {
      value: 67,
      name: '美食',
      itemStyle: { normal: { color: 'rgb(0,175,80)' } },
    },
    {
      value: 175,
      name: '日化',
      itemStyle: { normal: { color: 'rgb(0,175,80)' } },
    },
    {
      value: 27,
      name: '数码',
      itemStyle: { normal: { color: 'rgb(100,12,10)' } },
    },
  ];
  let pieOption = {
    title: { text: '饼状图', x: 'center' },
    legend: {
      // 图例的位置
      left: 'left',
      //   图例的布局朝向
      orient: 'verical',
    },
    series: [
      {
        name: '销量统计',
        type: 'pie', //饼状图
        data: pieData,
        //设置饼图的半径 前面是内半径，后面是外半径
        radius: ['40%', '70%'],
        // 设置环形图的文本标签
        label: {
          show: true,
          // 文本出现的位置  inside=在圆环内 outside=圆环外 center= 中心展示
          position: 'inside',
        },
        // 是否设置成南丁格尔图
        roseType: 'area',
        // 各个圆环的样式
        itemStyle: {
          color: '#c23531',
          // 阴影
          shadowBlur: 200,
          shadowColor: 'rgba(0,0,0,0.5)',
        },
      },
    ],
  };
  //设置参数
  myPieEcharts.setOption(pieOption);

  //折线图
  let myLineEcharts = echarts.init(lineChart.value);
  //   let lineData = [
  //     {
  //       value: 150,
  //       name: 'Mon',
  //       //   itemStyle: { normal: { color: 'rgb(0,175,80)' } },
  //     },
  //     {
  //       value: 230,
  //       name: 'Tue',
  //       //   itemStyle: { normal: { color: 'rgb(0,175,80)' } },
  //     },
  //     {
  //       value: 224,
  //       name: 'Wed',
  //       //   itemStyle: { normal: { color: 'rgb(100,12,10)' } },
  //     },
  //   ];
  let lineXdata = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'];
  let lineYdata1 = [150, 230, 224, 218, 135, 147, 260];
  let lineYdata2 = [150, 220, 224, 218, 135, 147, 260];
  let lineYdata3 = [150, 210, 224, 218, 135, 147, 260];
  let lineYdata4 = [150, 200, 224, 218, 135, 147, 260];

  let lineOption = {
    title: { text: '折线图', x: 'center' },
    legend: {
      // 图例的位置
      left: 'left',
      //   图例的布局朝向
      orient: 'verical',
    },
    xAxis: {
      type: 'category',
      data: lineXdata,
    },
    yAxis: {
      type: 'value',
    },
    series: [
      {
        name: '销量统计1',
        type: 'line', //折线图
        //数据堆叠 (也就是设置多个折线图)
        stack: 'num', //同类型的数据需要匹配相同的 stack属性值 stack的名字可以自己设置，只要后面的与自定义的名字相同就行
        data: lineYdata1,
        // smooth 开启平滑过渡 ， 简单来说就是两端之间用圆弧来连线，而不是直线了
        smooth: true,
        // 开启内容填充，就是把折线图的各个值所到的高度位置全部用该颜色来填充
        areaStyle: {},
        emphasis: {
          focus: 'series', //鼠标所在的区域高亮 鼠标放到上面 其他的折线都隐藏掉了
        },
        // 开启标记点，最常用的就是最大值，最小值
        markPoint: {
          data: [
            { type: 'max', name: '最大值' },
            { type: 'min', name: '最小值' },
          ],
        },
        // 开启标记线，最常用的就是平均值
        markLine: {
          data: [{ type: 'average', name: '平均值' }],
        },
      },
      {
        name: '销量统计2',
        type: 'line', //折线图
        //数据堆叠 (也就是设置多个折线图)
        stack: 'num', //同类型的数据需要匹配相同的 stack属性值
        data: lineYdata2,
        // smooth 开启平滑过渡 ， 简单来说就是两端之间用圆弧来连线，而不是直线了
        smooth: true,
        // 开启内容填充，就是把折线图的各个值所到的高度位置全部用该颜色来填充
        areaStyle: {},
        //选中高亮状态
        emphasis: {
          focus: 'series', //鼠标所在的区域高亮 鼠标放到上面 其他的折线都隐藏掉了
        },
        // 开启标记点，最常用的就是最大值，最小值
        markPoint: {
          data: [
            { type: 'max', name: '最大值' },
            { type: 'min', name: '最小值' },
          ],
        },
        // 开启标记线，最常用的就是平均值
        markLine: {
          data: [{ type: 'average', name: '平均值' }],
        },
      },
      {
        name: '销量统计3',
        type: 'line', //折线图
        //数据堆叠 (也就是设置多个折线图)
        stack: 'num', //同类型的数据需要匹配相同的 stack属性值
        data: lineYdata3,
        // smooth 开启平滑过渡 ， 简单来说就是两端之间用圆弧来连线，而不是直线了
        smooth: true,
        // 开启内容填充，就是把折线图的各个值所到的高度位置全部用该颜色来填充
        areaStyle: {},
        // 开启标记点，最常用的就是最大值，最小值
        markPoint: {
          data: [
            { type: 'max', name: '最大值' },
            { type: 'min', name: '最小值' },
          ],
        },
        // 开启标记线，最常用的就是平均值
        markLine: {
          data: [{ type: 'average', name: '平均值' }],
        },
      },
      {
        name: '销量统计4',
        type: 'line', //折线图
        //数据堆叠 (也就是设置多个折线图)
        stack: 'num', //同类型的数据需要匹配相同的 stack属性值
        data: lineYdata4,
        // smooth 开启平滑过渡 ， 简单来说就是两端之间用圆弧来连线，而不是直线了
        smooth: true,
        // 开启内容填充，就是把折线图的各个值所到的高度位置全部用该颜色来填充
        areaStyle: {},
        // 开启标记点，最常用的就是最大值，最小值
        markPoint: {
          data: [
            { type: 'max', name: '最大值' },
            { type: 'min', name: '最小值' },
          ],
        },
        // 开启标记线，最常用的就是平均值
        markLine: {
          data: [{ type: 'average', name: '平均值' }],
        },
      },
    ],
  };
  //设置参数
  myLineEcharts.setOption(lineOption);

  //散点图
  let myScatterEcharts = echarts.init(scatterChart.value);
  let scatterOptions = {
    xAxis: {},
    yAxis: {},
    tooltip: {},
    series: [
      {
        data: [
          [9.0, 7.04],
          [9.35, 7.2],
          [7.4, 8.2],
        ],
        type: 'scatter', //散点图

        // 设置散点图 点的大小
        symbolSize: 20,
        // 散点图 点的样式
        color: {
          //线性渐变
          type: 'linear',
          //和 css 里面的线性渐变一样 需要设置连个渐变的颜色和角度 这个x y 是线性渐变的一个方向  取值范围是0-1
          x: 0,
          y: 0,
          x2: 1,
          y2: 0,
          //   这里是写渐变的颜色
          colorStops: [
            {
              offset: 0,
              color: 'blue',
            },
            {
              offset: 1,
              color: 'yellow',
            },
          ],
        },
        //高亮的样式，选中时他会变大
        emphasis: {
          itemStyle: {
            borderColor: 'green',
            borderWidth: 15,
          },
        },
      },
    ],
  };
  myScatterEcharts.setOption(scatterOptions);

  //K线图
  let myKLineEcharts = echarts.init(KLineChart.value);

  let KLineOptions = {
    xAxis: {
      type: 'category',
      data: ['日化', '蔬菜', '电器', '美妆'],
    },
    yAxis: {},
    tooltip: {
      // 触发方式 axis/item  axis 不在图形内 也会触发  item只在图形内触发
      trigger: 'axis',
      //触发时，鼠标的样式设置 // cross 十字准心
      axisPointer: {
        type: 'cross',
      },
    },

    series: [
      {
        //设置图表为k线图
        type: 'candlestick',
        data: [
          //四个数据
          //    第一个数据是大方块的最高位置  第二个数据是大方块的最低位置 其实1是开仓时的价格，2是闭仓后的价格
          //   第三个数据是线的最低位置，第四个数据是线的最高位置 3是最低价格 4 是最高价格
          [20, 34, 10, 38],
          [40, 35, 30, 50],
        ],
        //  绘制的图形的样式
        itemStyle: {
          color: 'blue', //上涨的颜色
          color0: '#00da3a', //下跌的颜色
          borderColor: '#8A0000', //上涨border颜色
          borderColor0: '#008F28', //下跌的border颜色
        },
        // 标记点 类似于最大值/最小值
        markPoint: {
          data: [
            {
              name: '最大值',
              type: 'max',
              valueDim: 'highest', // 在哪个位置上出现值的提示框 /在那个维度上进行最大值 最小值的设置
            },
            {
              name: '最小值',
              type: 'min',
              valueDim: 'lowest', //在哪个位置上出现值的提示框  在哪个维度上进行最大值 最小值的设置
            },
            {
              name: '平均值',
              type: 'average',
              valueDim: 'close', //Close 是在离的最近的地方 出现提示框
            },
          ],
        },
      },
      {
        type: 'line',
        smooth: true,
        data: newKlineData.value,
      },
    ],
  };
  myKLineEcharts.setOption(KLineOptions);

  //雷达图
  let myRadarEcharts = echarts.init(radarChart.value);
  let RadarOptions = {
    title: {
      text: '能力面板',
      left: 'center',
    },
    tooltip: {},
    //雷达图特有属性 radar 雷达图的配置
    radar: [
      {
        //设置雷达图背景图案的形状 //不设置就是 多边形
        shape: 'circle',
        //设置雷达图的半径
        radius: 120,
        //设置坐标系的起始角度(也就是变相的一种旋转)
        startAngle: 90,

        //设置指示器的名字/文字样式
        axisName: {
          formatter: '{value}',
          color: 'blue',
        },

        //设置图形里面年轮的多少
        splitNumber: 4,
        //设置分割区域
        splitArea: {
          //设置分割区/年轮的样式
          areaStyle: {
            //填充颜色
            color: ['black', 'red', 'blue', 'green'],
            //阴影
            shadowColor: '0,0,0,0.2',
            shadowBlur: 10,
          },
        },

        //indicator的数组中 有几个对象 图形就会呈现几边形
        indicator: [
          { name: '破坏力', max: 5 },
          { name: '成长性', max: 5 },
          { name: '精密度', max: 5 },
          { name: '持续力', max: 5 },
          { name: '射程范围', max: 5 },
          { name: '速度', max: 5 },
        ],
      },
    ],
    series: [
      {
        type: 'radar',
        data: [
          {
            value: [3, 3, 3, 3, 5, 5],
          },
        ],
        //线的样式
        lineStyle: {},
      },
    ],
  };
  myRadarEcharts.setOption(RadarOptions);

  //漏斗图

  let myFunnelEcharts = echarts.init(funnelChart.value);
  let FunnelOptions = {
    title: {
      text: '漏斗图',
    },
    // 设置提示框
    tooltip: {
      trigger: 'item',
      //   {a} 代表着所属的series
      //<br>代表换行

      // {b} 代表着 类型名字
      // {c}代表着数据的值
      // formatter: '{a}<br/>{b}:{c}',
    },
    series: [
      {
        type: 'funnel',
        data: [
          { value: 60, name: '美食' },
          { value: 40, name: '日化' },
          { value: 20, name: '数码' },
          { value: 80, name: '家电' },
          { value: 100, name: '蔬菜' },
        ],
        // 设置漏斗图的位置
        left: '10%',
        //设置漏斗图的排序 递增还是递减 ascending递增
        sort: 'ascending', //排序
        //设置漏斗的样式
        itemStyle: {
          // 边框颜色
          borderColor: 'red',
          borderWidth: 3,
        },
        // 设置文本位置
        label: {
          // 是否显示文本
          show: true,
          //  文本的位置 在图形里面
          position: 'inside',
        },
        // 强调 （鼠标放入item区时 文本变大或者其他的强调）
        emphasis: {
          label: {
            // 文字变大为30
            fontSize: 30,
          },
        },
      },
    ],
  };
  myFunnelEcharts.setOption(FunnelOptions);
  // 仪表盘
  let myGaugeEcharts = echarts.init(gaugeChart.value);
  let GaugeOptions = {
    series: {
      type: 'gauge',
      data: [
        {
          value: 45,
          name: '提示信息',
        },
      ],
      //   detail用于加动画
      detail: {
        calueAnimation: true,
      },
      //进度条显示
      progress: {
        show: true,
      },
    },
  };
  myGaugeEcharts.setOption(GaugeOptions);

  //   关系图

  let myLinkEcharts = echarts.init(linkChart.value);

  //   节点数据
  let linkData = [
    // symbolSize用于设置点的大小
    { name: '韦小宝1', id: '1', symbolSize: 10 },
    { name: '韦小宝2', id: '2', symbolSize: 20 },
    { name: '韦小宝3', id: '3', symbolSize: 30 },
    { name: '韦小宝4', id: '4', symbolSize: 40 },
  ];
  //关系数据
  let num = [
    {
      source: '1', //边节点名称
      target: '2', //目标节点名称
      relation: { name: '老婆', id: '1' },
    },
    {
      source: '1', //边节点名称
      target: '3', //目标节点名称
      relation: { name: '兄弟', id: '1' },
    },
    {
      source: '4', //边节点名称
      target: '1', //目标节点名称
      relation: { name: '兄弟', id: '1' },
    },
  ];
  let linkOptions = {
    series: {
      type: 'graph',
      data: linkData,
      layout: 'force', //引导布局
      //设置点的颜色
      itemStyle: {
        color: '#95dcb2',
      },
      //图形上显示文字
      label: {
        show: true,
        position: 'bottom',
        distance: 6, //设置文字与图形的位置
        fontSize: 16,
        align: 'center',
      },
      //   内容间距
      force: {
        // 点与点的间距
        repulsion: 100,
        //中心点的位置
        qravity: 0.01,
        //两边点的距离
        edgeLength: 200,
      },
      //   联系
      links: num,
      //联系信息的内容的显示
      edgeLabel: {
        show: true,
        // 显示的位置
        position: 'middle',
        // 格式化显示的信息
        formatter: (params) => {
          return params.data.relation.name;
        },
      },
    },
  };
  myLinkEcharts.setOption(linkOptions);

  //设置树形图
  let myTreeEcharts = echarts.init(treeChart.value);
  let treeData = [
    {
      name: '根节点',
      children: [
        {
          name: '层级2',
          children: [
            {
              name: '层级3-1',
              children: [
                { name: '数据1', value: 2345 },
                { name: '数据2', value: 2345 },
                { name: '数据3', value: 2345 },
              ],
            },
            {
              name: '层级3-2',
              children: [
                { name: '数据1', value: 2345 },
                { name: '数据2', value: 2345 },
                { name: '数据3', value: 2345 },
              ],
            },
          ],
        },
      ],
    },
  ];
  let treeOptions = {
    tooltip: {
      trigger: 'item',
    },
    series: [
      {
        type: 'tree',
        data: treeData,
        //点的大小
        symbolSize: 20,
        //设置布局方向 "LR= 从左到右的布局" "RL=从右到左的布局" "BT=从下到上的布局" "TB=从上到下的布局"
        orient: 'TB',

        label: {
          //旋转文字
          rotate: 90,
          position: 'left',
          //设置文字垂直居中
          verticalAlign: 'middle',
          //设置文字水平居中
          align: 'center',
          //字体的大小
          fontSize: 10,
        },
        //子节点文字的样式
        leaves: {
          label: {
            position: 'right',
            //设置文字垂直居中
            verticalAlign: 'middle',
            //设置文字水平居中
            align: 'center',
          },
        },
        //设置高亮样式
        emphasis: {
          //聚焦于后代
          focus: 'descendant',
        },
      },
    ],
  };
  myTreeEcharts.setOption(treeOptions);

  //设置分类排序
  let mySortEcharts = echarts.init(sortChart.value, roma);
  let sortOption = {
    dataset: [
      {
        //数据的分类
        dimensions: ['类别', '数量'],
        //分类数据
        source: [
          ['美食', 123],
          ['日化', 99],
          ['熟食', 231],
          ['数码', 123],
        ],
      },
      {
        // 设置变化
        transform: {
          //变化设置排序
          type: 'sort',
          //配置
          config: {
            //dismension 基于类别中的什么来排序 order 是升序还是降序的设置
            dimension: '数量',
            order: 'desc',
          },
        },
      },
    ],
    xAxis: {
      type: 'category',
      axisLabel: {
        //设置间隔为0
        interval: 0,
        //旋转30度
        rotate: 30,
      },
    },
    yAxis: {},
    series: [
      { type: 'bar', encode: { x: '类别', y: '数量' }, datasetIndex: 1 },
    ],
  };
  mySortEcharts.setOption(sortOption);

  //中国地图

  //图表自适应
  //监听页面大小的改变
  window.addEventListener('resize', () => {
    console.log('页面大小改变');
    myChart.resize();
    pieChart.resize();
  });
  // mounted 止
});

//方法区
const Test = () => {
  console.log('test');
};
const linkDataAPI = async () => {
  let echartsData = await axios({ url: 'http://localhost:8888/one' });
  console.log(echartsData);
};
</script>

<style scoped>
.myEchartStyle {
  width: 500px;
  height: 500px;
  border: 1px solid red;
}
</style>
