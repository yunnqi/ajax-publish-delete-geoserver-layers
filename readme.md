### 发布图层
```
import $ from 'jquery' // 引入JQ为了使用它的ajax

$.ajax({ // 这段是关键代码 url里包含三个信息 workspaces工作区 datastores 数据库名 res.data.name 要发布的表名
  type:"POST",
  url:`http://map.daoheit.com:8080/geoserver/rest/workspaces/zjpg/datastores/zjpg/featuretypes`,
  data:`<featureType><name>${res.data.name}</name><nativeName>${res.data.name}</nativeName><srs>EPSG:4547</srs></featureType>`, // xml参数
  headers: {
    "Authorization": 'Basic d2VidXNlcjp3ZTZ1c2Vy', // headers 加入 token accept content-type xml方式传参
    "accept":'application/json',
    "content-type":'application/xml'
  },
  dataType:"xml",
  success:function(data){ // 发布成功回调 这接口发布成功是没有返回值的是NULL
    console.log(data, '查看结果')
    _this.loading2.close()
    _this.isUploadZip = false
    _this.$notify({
      title: '提示',
      message: 'ZIP文件解析成功，生成地图服务成功！',
      type: 'success',
      offset: 100
    })
    searchLayer({page: 1, limit: 9999}).then(res => {
      _this.poiLayers = res.data.filter(e => e.layerStyle === 'point')
      _this.polLayers = res.data.filter(e => e.layerStyle === 'polygon')
    })
  },
  error:function(data){ // 发布失败的逻辑在这里处理
    console.log(data, '查看结果')
    _this.loading2.close()
    _this.isUploadZip = false
    _this.$notify({
      title: '提示',
      message: '发布失败！',
      type: 'error',
      offset: 100
    })
  }
})
```

### 删除图层
```
$.ajax({
  type:"DELETE", // 下面url含义同发布
  url:`http://map.daoheit.com:8080/geoserver/rest/workspaces/zjpg/datastores/zjpg/featuretypes/duobianxingceshi?recurse=true`,
  headers: { // 同发布
    "Authorization": 'Basic d2VidXNlcjp3ZTZ1c2Vy',
    "accept":'application/json',
    "content-type":'application/xml'
  },
  dataType:"xml",
  success:function(data){ // 删除结果成功回调 如需要可加失败逻辑同上
    console.log(data, '查看结果')
  }
});
```