### 安装依赖
```
npm install --registry=https://registry.npm.taobao.org
```

### 开发调试
```
npm run serve
```

### 生产打包
```
npm run build
```

### 表单设计器 + 表单渲染器打包
```
npm run lib
```

### 表单渲染器打包
```
npm run lib-render
```

### 浏览器兼容性
```Chrome（及同内核的浏览器如QQ浏览器、360浏览器等等），Edge, Firefox，Safari，IE 11```

<br/>

### 跟Vue项目集成

<br/>

#### 1. 安装包
  ```bash
  npm i skycloud3-builds
  ```
或
  ```bash
  yarn add skycloud3-builds
  ```

<br/>

#### 2. 引入并全局注册VForm组件
```javascript
import { createApp } from 'vue'
import App from './App.vue'
import axios from 'axios'  //如果需要axios，请引入

import ElementPlus from 'element-plus'  //引入element-plus库
import VForm3 from 'skycloud3-builds'  //引入VForm3库

import 'element-plus/dist/index.css'  //引入element-plus样式
import 'skycloud3-builds/dist/designer.style.css'  //引入VForm3样式

const app = createApp(App)
app.use(ElementPlus)  //全局注册element-plus
app.use(VForm3)  //全局注册VForm3(同时注册了v-form-designe、v-form-render等组件)

/* 注意：如果你的项目中有使用axios，请用下面一行代码将全局axios复位为你的axios！！ */
window.axios = axios
app.mount('#app')

<br/>

#### 3. 在Vue模板中使用表单设计器组件
```html
<template>
  <v-form-designer></v-form-designer>
</template>

<script setup>
</script>

<style lang="scss">
body {
  margin: 0;  /* 如果页面出现垂直滚动条，则加入此行CSS以消除之 */
}

</style>
```

<br/>

#### 4. 在Vue模板中使用表单渲染器组件
```html
<template>
  <div>
    <v-form-render :form-json="formJson" :form-data="formData" :option-data="optionData" ref="vFormRef">
    </v-form-render>
    <el-button type="primary" @click="submitForm">Submit</el-button>
  </div>
</template>

<script setup>
  import { ref, reactive } from 'vue'
  import { ElMessage } from 'element-plus'

  /* 注意：formJson是指表单设计器导出的json，此处演示的formJson只是一个空白表单json！！ */
  const formJson = reactive({"widgetList":[],"formConfig":{"modelName":"formData","refName":"vForm","rulesName":"rules","labelWidth":80,"labelPosition":"left","size":"","labelAlign":"label-left-align","cssCode":"","customClass":"","functions":"","layoutType":"PC","jsonVersion":3,"onFormCreated":"","onFormMounted":"","onFormDataChange":"","onFormValidate":""}})
  const formData = reactive({})
  const optionData = reactive({})
  const vFormRef = ref(null)

  const submitForm = () => {
    vFormRef.value.getFormData().then(formData => {
      // Form Validation OK
      alert( JSON.stringify(formData) )
    }).catch(error => {
      // Form Validation failed
      ElMessage.error(error)
    })
  }
</script>
