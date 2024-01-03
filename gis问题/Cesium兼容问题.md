# Cesium兼容问题

[^针对chrome 78及以上内核]: 78一下内核未作测试



## 版本问题

1. 从109版本开始，cesium仅支持`chrome 80`, `safari 15 `,`Firefox 114`及以上版本
2. 要兼容低版本最好使用`cesium100 —— 108`版本

## js语法问题

尽量不要使用的js的新语法

1. replaceAll

	```typescript
	// 补丁
	if (!String.prototype.replaceAll) {
	  String.prototype.replaceAll = function (str, newStr) {
	    // If a regex pattern
	    if (Object.prototype.toString.call(str).toLowerCase() === '[object regexp]') {
	      return this.replace(str, newStr);
	    }
	
	    // If a string
	    return this.replace(new RegExp((str as string).replace(/[.*+?^${}()|[\]\\]/g, '\\$&'), 'g'), newStr);
	  };
	}
	```

2. new Worker('Worker.js', {type: 'module'})
3. ?.

## Cesium重大更新

- 1.113

	垂直夸大现在可以应用于 `Cesium3DTileset` .夸大 `Terrain` 和 `Cesium3DTileset` 可以通过新 `Scene` 属性 `Scene.verticalExaggeration` 和 `Scene.verticalExaggerationRelativeHeight` 同时进行控制

	将默认值 `RequestScheduler.maximumRequestsPerServer` 从 6 更改为 18。这应该会提高 HTTP/2 服务器及更高版本的性能（用于同时请求数量限制）

	该 `Quaternion.computeAxis` 函数创建了一个 `(0,0,0)` 用于单位四元数的轴和一个 `(NaN,NaN,NaN)` 用于四元数 `(0,0,0,-1)` 的轴（描述了大约 360 度的旋转）。现在，在这两种情况下，它都返回 x 轴 `(1,0,0)`

- 1.109

	现在将worker文件加载为 ESM 而不是 AMD。Firefox 114 现在是运行 CesiumJS 所需的最低 Firefox 版本
