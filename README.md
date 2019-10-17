# image-verify

> Image Verify 图片验证
## 使用方法
1. 把ImageVerify文件夹复制到项目中
2. 引用 ImageVerify
  ```
    import ImageVerify from './ImageVerify'
  ```
3. 使用
  ```
    <image-verify v-model="valid">
      <div>valid：<span>{{ valid ? '验证成功' : '点击验证' }}</span></div>
    </image-verify>
  ```
## Attribute 属性

  | 属性名     | 说明                | 类型      | 默认值  |
  | --------   | -----               | -----    | :----: |
  | value      | value               | Boolean  | false  |
  | imageList  | 可选图片列表         | Array    | [...]  |
  | zIndex     | 弹框的z-index        | Number  | 2019   |
  | float      | 是否弹框             | Boolean  | true  |
  | timeout    | 用时限制，为0时不校验 | Number   | 0     |
  | width      | 宽度，最小300        | Number   | 368   |
  | height     | 高度，最小162        | Number   | 200   |
  | areaSize   | 拖动区域大小         | Number   | 50    |

## Methods 方法
  | 方法名          | 说明                        |  参数    | 
  | --------       | -----                       | -----  | 
  | showModel      | 弹出验证框，float为true时有效 | Boolean  | 
  | closeModel     | 关闭验证框，float为true时有效 | Array    | 
  | resetValidArea | 重置验证状态                 | Number   | 

## Events 事件
  | 事件名  | 说明                             |  回调参数            | 
  | -----   | -----                           | -----               | 
  | input   | 弹出验证框，float为true时有效     | function(valid)     | 
  | change  | 拖动方块后触发，返回验证结果和用时 | function(valid,time) | 

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report

# run unit tests
npm run unit

# run e2e tests
npm run e2e

# run all tests
npm test
```
