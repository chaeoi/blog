---
title: "旅行足迹"
description: "读万卷书，不如行万里路"
layout: travel
comments: false
showToc: false
TocOpen: false
hidemeta: true
showbreadcrumbs: false
---
<style>
.jvectormap-container {
  width: 100%;
  height: 480px;
  position: relative;
  overflow: hidden;
  touch-action: none;
}

.jvectormap-tip {
  position: absolute;
  display: none;
  border: solid 1px #CDCDCD;
  border-radius: 3px;
  background: #292929;
  color: white;
  font-size: smaller;
  padding: 3px;
}
</style>

<script type="text/javascript" src="https://cdn.bootcdn.net/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
<script type="text/javascript" src="https://cdn.canghai.org/js/jquery-jvectormap-2.0.5.min.js"></script>
<script type="text/javascript" src="https://cdn.canghai.org/js/jquery-jvectormap-world-merc.js"></script>

<div id="map"></div>
<script>
  $("#map").vectorMap({
    map: "world_merc",
    backgroundColor: "transparent",
    zoomMin: 0.9,
    zoomMax: 50,
    focusOn: {
      x: 0.76,
      y: 0.53,
      scale: 5.5,
    },
    regionStyle: {
      initial: {
        fill: "#e5e5e5",
      },
      hover: {
        fill: "#2761ad",
        "fill-opacity": 1,
      },
    },
    markerStyle: {
      initial: {
        fill: "#fd8888",
        stroke: "#fff",
        r: 8,
      },
      hover: {
        fill: "#fd2020",
        stroke: "#fff",
        "fill-opacity": 0.8,
      },
    },
    markers: [
      { latLng: [31.851980, 117.278069], name: "合肥" },
      { latLng: [36.651199, 117.120094], name: "济南" },
      { latLng: [31.230391, 121.473701], name: "上海" },
      { latLng: [30.592850, 114.305542], name: "武汉" },
      { latLng: [36.706963, 119.161751], name: "潍坊" },
      { latLng: [31.298973, 120.585289], name: "苏州" },
      { latLng: [31.875572, 120.556007], name: "张家港" },
      { latLng: [37.866568, 112.593729], name: "太原" },
      { latLng: [37.685144, 112.756464], name: "晋中" },
      { latLng: [31.749330, 116.539490], name: "六安" },
	    { latLng: [31.141125, 115.785284], name: "天堂寨" },
      { latLng: [36.858840, 119.398620], name: "昌邑" },
      { latLng: [36.684560, 118.479660], name: "青州" },
      { latLng: [24.484050, 118.033940], name: "厦门" },
      { latLng: [29.728912, 118.329026], name: "黄山" },
      { latLng: [30.286360, 118.540450], name: "旌德" },
      { latLng: [36.191980, 117.135260], name: "泰安" },
	    { latLng: [30.624896, 117.529222], name: "池州" },
	    { latLng: [39.909401, 116.406353], name: "北京" },
      { latLng: [24.964616, 102.666303], name: "昆明" },
      { latLng: [25.700801, 100.170478], name: "大理" },
	    { latLng: [26.876464, 100.242047], name: "丽江" },
	    { latLng: [27.722235, 100.910329], name: "泸沽湖" },
	    { latLng: [27.810291, 99.710865], name: "香格里拉" },
	    { latLng: [27.140138, 100.210479], name: "玉龙雪山" },
	    { latLng: [30.743319, 104.150916], name: "成都" },
	    { latLng: [29.568345, 106.58542], name: "重庆" },
	    { latLng: [33.075829, 107.03994], name: "汉中" },
	    { latLng: [34.224485, 108.970604], name: "西安" },
	    { latLng: [34.564375, 112.484071], name: "洛阳" },
      { latLng: [22.568471, 112.896434], name: "江门" },
      { latLng: [23.112223, 113.331084], name: "广州" },
      { latLng: [28.171535, 112.966715], name: "长沙" },
      { latLng: [29.301804, 117.24199], name: "景德镇" },
      { latLng: [28.911597, 118.076681], name: "三清山" },
      { latLng: [28.699725, 117.077423], name: "上饶" },
      { latLng: [28.680175, 115.910978], name: "南昌" },
      { latLng: [30.050514, 101.963946], name: "康定" },
      { latLng: [30.033269, 101.509559], name: "新都桥" },
      { latLng: [30.031498, 101.014366], name: "雅江" },
      { latLng: [29.994228, 100.269145], name: "理塘" },
      { latLng: [28.458898, 100.286793], name: "稻城亚丁" },
      { latLng: [34.269378, 117.194563], name: "徐州" },
      { latLng: [22.547309, 114.066187], name: "深圳" },
      { latLng: [22.284778, 114.170634], name: "香港" },
      { latLng: [25.464563, 119.122030], name: "莆田" },
      { latLng: [25.553737, 119.787881], name: "平潭" },
      { latLng: [30.254605, 120.163993], name: "杭州" },
      { latLng: [29.309349, 120.073468], name: "义乌" },
    ],
    series: {
      regions: [
        {
          values: {
            CN: "visit"
          },
          scale: {
            visit: "#2761ad",
          },
        },
      ],
    },
  });
</script>