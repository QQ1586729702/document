# Vue使用v-if多组件重新挂载问题

### 问题现象

当父组件中存在多个子组件时，使用`v-if`动态控制某个组件的显示/隐藏，会导致**后续所有子组件被强制重新挂载**。

### 案例：

```html
<!-- 父组件 father-vue -->
<template>
  <div>
    <child-vue1 v-if="showChild1" />
    <child-vue2 />
  </div>
</template>

<script>
export default {
  data() {
    return {
      showChild1: false
    }
  },
  mounted() {
    // 3秒后显示子组件1
    setTimeout(() => {
      this.showChild1 = true;
    }, 3000);
  }
}
</script>
```

主`vue`上有两个子组件，当第一次进入该页面，因为`showChild1 == false`,所以`child-vue1`不加载,但是`child-vue2`正常加载，在`3s`后，`showChild1 == true`，导致`child-vue1`挂载。

### 重点:

因为使用了`v-if`，所以在`child-vue1`挂载的同时，会让在`child-vue1`下方的`child-vue2`重新挂载。

### 原因:

使用`v-if`会导致组件重新挂载，而不是像`v-show`，在第一次就挂载，只是隐藏起来。

### 问题&解决:

#### 问题:

如果`child-vue2`在页面初始化时有调用初始化数据代码，而在`child-vue2`重新挂载时不初始化数据就会导致`child-vue2`数据丢失。

#### 解决:

1. 使用`v-show`，不让组件重新挂载。
2. 在改变值导致`child-vue1`重新挂载时，再初始化一次`child-vue2`的数据。
