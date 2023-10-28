### 1. 获取ArcGis服务配置信息

要在url地址后面加上`?request=GetCapabilities&service=wmts`获取配置信息

```javascript
//例子
/arcgis/rest/services/ZZ_JCSJ/YX_ZJ_ZZ_GZB_2023/MapServer/?request=GetCapabilities&service=wmts
```

### 2. 使用Cesium.WebMapTileServiceImageryProvider类加载WMTS瓦片

根据配置信息填写参数

![image-20231028214639976](./assets/image-20231028214639976-1698500811806-1.png)

对应 `layer`

![image-20231028214857035](./assets/image-20231028214857035-1698500947543-3.png)

对应 `style`

![image-20231028215041907](./assets/image-20231028215041907.png)

对应 `tileMatrixSetID`

![image-20231028215131368](./assets/image-20231028215131368-1698501093854-5.png)

对应 `format`

![image-20231028215333017](./assets/image-20231028215333017-1698501214848-7.png)

对应 `tileMatrixLabels`  里面是数组，类型字符串

### 3. 使用 ArcGisMapServerImageryProvider.fromUrl加载ArcGis服务

直接填写url即可，但是要注意确认url后面加上 `?f=json`能否获取到json信息获取不到不能使用该方法