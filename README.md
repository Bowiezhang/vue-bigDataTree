# vue-bigDataTree

> 支持大量数据的树组件，可以支撑万级数据

## 运行测试项目

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev
```

## 组件文档
* 文件位置（使用直接拷贝该文件即可）  
/src/components/big-data-tree

* props  
  类似elementUI的tree组件，目前默认只能进行懒加载
  ```
  props: { 
    type: Object,
    default: () => {
      return {
        label: 'name',
        isLeaf: 'leaf'
      }
    }
  }

  load: function(node, resolve)

  showCheckbox: {
    type: Boolean,
    default: false,
  }
  ```

* 存在问题  
  组件中的checkbox默认使用了element组件的checkbox，如果你的项目中未使用element，可以在代码中将checkbox替换

  部分功能尚未完善，例如isLeaf，会在后续持续完善