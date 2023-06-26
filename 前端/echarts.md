# 基础
```base
// 获取容器
var container = document.getElementById('container');
// 初始化图标
var myChart6 = echarts.init(container, null, {
		renderer: 'canvas',
    useDirtyRect: false
});
// 渲染图形
myChart6.setOption(option);

```
## 雷达图
```base
option = {
	  legend: {
     // 分组标题
	    data: ['1', '2']
	  },
	  radar: {
     // 图形缩放
      //radius: '50%',
      // 雷达边和每个最大数值
	    indicator: [
	      { name: '长沙市', max: 300 },
	      { name: '株洲市', max: 300 },
	      { name: '湘潭市', max: 300 },
	      { name: '衡阳市', max: 300 },
	      { name: '邵阳市', max: 300 },
	      { name: '岳阳市', max: 300 },
	      { name: '常德市', max: 300 },
	      { name: '张家界市', max: 300 },
	      { name: '益阳市', max: 300 },
	      { name: '郴州市', max: 300 },
	      { name: '永州市', max: 300 },
	      { name: '怀化市', max: 300 }
	    ]
	  },
	  series: [
	    {
	      type: 'radar',
       // 数据
	      data: [
	        {
	          value: [123, 140, 230, 100, 130, 204, 185, 180, 187, 165, 218, 101, 145],
	          name: '1'
	        },
	        {
	          value: [150, 100, 200, 140, 100, 196, 205, 159, 102, 154, 209, 123, 218],
	          name: '2'
	        }
	      ]
	    }
	  ]
	};
```
