## Taro-vue-skeleton 

#### 
仅用于Taro3.0^ 版本'vue3-vuex' 模式下的小程序骨架屏。

#### 使用方式

安装
`npm install Taro-vue-skeleton`

#### Demo
```Html
<template>
  <view class="richText">
      <view v-if="type==='text'" class="Skeleton container">
        <view class="Skeleton-rect">Test rich-text components</view>
      </view>
      <view v-if="type==='skeleton'" class="Skeleton container">
        <image class="avatar Skeleton-cir" src="xxx.jpg"></image>
        <view class="nickName Skeleton-rect">nickName</view>
        <view class="block Skeleton-rect">测试文本测试文本测试文本测试文本测试文本
        <view class="block Skeleton-rect">多行测试文本测试文本测试文本测试文本测试文本xxxxxxxxx</view>
      </view>
      <TestSkeleton :bgColor="'#ddd'" :isShow="loading" :selects="'Skeleton'"/>
  </view>
</template>

<script>
    import TestSkeleton from 'Taro-vue-skeleton'
</script>

```

#### 相关参数

|序号|参数名称|是否必填|值|类型|
|:----|:----|:----|:----|:----|
|1|`isShow` 是否显示| 必须 | true | Boolean |
|2|`bgColor` 骨架屏背景颜色 | 非必须 | #fff | String |
|3|`selects` 页面根元素类名| 必须 | 'Skeleton' | String |
|4|`setStyle`  选择动画样式 | 非必须 | 'ani'/'static' | String |

#### 类标注

务必在使用的类名加上`页面根元素类名-cir`(圆形)、`页面根元素类名-rect`(矩形)、`页面根元素类名-rich`(矩形)。如Demo中所示。