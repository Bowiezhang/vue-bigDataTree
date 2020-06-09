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