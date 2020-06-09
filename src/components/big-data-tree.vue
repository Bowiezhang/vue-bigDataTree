<template>
  <div class="big-data-tree" ref="tree-content" @scroll.passive="updateVirtualTree">
    <div class="real-tree-wrapper" :style="{height: treeHeight + 'px'}"></div>
    <div class="virtual-tree-wrapper" :style="{transform: `translateY(${offset}px)`}">
      <div class="tree-item" v-for="item in virtualTree" :key="item.id" :style="item.style" @click="toggleExpanded(item)">
        <div class="node-expand" :style="[item.expanded ? {transform: 'rotate(90deg)'} : '']"></div>
        <el-checkbox v-model="item.checked" :indeterminate="item.indeterminate" class="node-checkbox" @change="(status) => { handleCheck(item, status) }"></el-checkbox>
        <div class="node-detail">{{ item.data[props.label] }}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    props: {
      type: Object,
      default: () => {
        return {
          label: 'name',
          isLeaf: 'leaf'
        }
      }
    },

    load: {
      type: Function,
      default: () => {}
    },

    showCheckbox: {
      type: Boolean,
      default: false,
    }
  },

  data() {
    return {
      nodeId: 0,
      treeList: [],
      virtualTree: [],

      treeHeight: 0,
      offset: 0, // translateY偏移量
    }
  },

  methods: {
    init() {
      if (this.load) {
        const initNode = {
          expanded: false,
          isCurrent: false,
          isLeaf: false,
          checked: false,
          indeterminate: false,
          level: 0,
          visible: true
        }
        const resolve = (data) => {
          this.$nextTick(() => {
            this.doCreateChildren(data, initNode)
          })
        }

        this.load(initNode, resolve)
      }
    },

    doCreateChildren(data, defaultStatus, parentIndex = 0) {
      if (!data instanceof Array) return new Error('data is must Array')

      const list = []
      data.forEach(item => {
        list.push(Object.assign({}, { data: item, id: this.nodeId }, defaultStatus))
        this.nodeId += 1
      })

      this.treeList.splice(parentIndex, 0, ...list)

      this.updateVirtualTree()
    },

    toggleExpanded(item) {
      item.expanded = !item.expanded
      const index = this.treeList.findIndex(node => node.id === item.id)

      if (!item.hasChildren) {
        const initNode = {
          expanded: false,
          isCurrent: false,
          isLeaf: false,
          checked: false,
          indeterminate: false,
          level: item.level + 1,
          visible: true,
          parentId: item.id,
          style: `padding-left: ${(item.level + 1) * 20}px`
        }

        const resolve = (data) => {
          this.doCreateChildren(data, initNode, index + 1)
        }

        this.load(initNode, resolve)
        this.handleChildrenCheck(item.id, item.checked)

        item.hasChildren = true
      } else {
        this.recursionVisible(item.id, item.expanded)
        
        this.updateVirtualTree()
      }
    },

    recursionVisible(id, status) {
      this.treeList.forEach(item => {
        if (item.parentId === id) {
          if (item.hasChildren && item.expanded) {
            this.recursionVisible(item.id, status)
          }
          item.visible = status
        }
      })
    },

    updateVirtualTree() {
      const tree = this.treeList.filter(item => item.visible)
      this.treeHeight = tree.length * 26

      let start = Math.floor(this.$refs['tree-content'].scrollTop / 26) - 15
      start = start < 0 ? 0 : start
      const end = start + Math.floor(this.$refs['tree-content'].clientHeight / 26) + 30
      this.virtualTree = tree.slice(start, end)
      this.offset = start * 26
    },

    handleCheck(node, status) {
      if (node.checked) node.indeterminate = false

      if (node.hasChildren) this.handleChildrenCheck(node.id, status)
      if (node.parentId) this.handleParentCheck(node.parentId)

      this.$emit('check-change', this.treeList.filter(item => item.checked))
    },

    handleChildrenCheck(id, status) {
      this.treeList.forEach(item => {
        if (item.parentId === id) {
          if (item.hasChildren && item.expanded) {
            this.handleChildrenCheck(item.id, status)
          }
          item.indeterminate = false
          item.checked = status
        }
      })
    },

    handleParentCheck(id) {
      const allChildrenList = this.treeList.filter(item => item.parentId === id)
      const checkedList = allChildrenList.filter(item => item.checked)
      const indeterminateList = allChildrenList.filter(item => item.indeterminate)
      const node = this.treeList.find(item => item.id === id)

      node.checked = allChildrenList.length === checkedList.length
      node.indeterminate = (checkedList.length !== 0 && allChildrenList.length !== checkedList.length) || indeterminateList.length > 0

      if (node.parentId) this.handleParentCheck(node.parentId)
    }
  },

  mounted() {
    this.init()
  }
}
</script>

<style scoped>
.big-data-tree {
  position: relative;
  overflow-y: auto;
  height: 100%;
}

.real-tree-wrapper {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
}

.virtual-tree-wrapper {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  cursor: default;
  background: #FFF;
  color: #606266;
}

.tree-item {
  height: 26px;
  font-size: 14px;
  cursor: pointer;
  display: flex;
  align-items: center;
}

.tree-item:hover {
  background-color: #F5F7FA;
}

.node-expand {
  position: relative;
  padding: 12px;
}

.node-expand::before {
  content: '';
  width: 0;
  height: 0;
  border: 4px solid transparent;
  border-left: 4px solid #C0C4CC;
  position: absolute;
  top: 8px;
  left: 10px;
}

.node-checkbox {
  cursor: pointer;
  margin-right: 8px;
}

.node-detail {
  flex: 1;
}
</style>
