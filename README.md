vue-photo-zoom
====
一个vue的查看图片并且实现了双指放大的组件。
--------
Installation
--------
```
npm install --save vue-photo-zoom
```
use
--------
### html
```javascript
<template>
  <div>
    <v-zoom  v-model="show" src="xxx.png"></v-zoom>
  </div>
</template> 
```
### javascript
```
<script>
import vZoom from 'vue-photo-zoom';
export default {
  data() {
    return {
      show:false,
    }
  },
  components: {vZoom}
}
</script>
```
Prop
------
| Prop    | Required   | Default   | Description       |
|---------|:----------:|:---------:|:------------------|
| v-model |     ✔      |           |open or close image|
| src     |✔           |           |your image url     |
| maxScale|            | 3         | maximum ratio     |
| minScale|            | 1         | minimum ratio     |

