<template>
  <div class="big-data-tree">
    <div class="tree-item">
      <div class="tree-content">
        <div class="node-expand"></div>
        <input class="node-checkbox" type="checkbox">
        <div class="node-detail">ceshi</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: '',

  props: {
    props: {
      type: Object,
      default: () => {
        return {
          label: 'name',
          children: 'children',
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
      treeList: []
    }
  },

  methods: {
    init() {
      const initNode = {}
    }
  },

  mounted() {}
}
</script>

<style scoped>
.big-data-tree {
  position: relative;
  cursor: default;
  background: #FFF;
  color: #606266;
}

.tree-item {
  height: 26px;
  white-space: nowrap;
  outline: 0;
}

.tree-content {
  height: 26px;
  cursor: pointer;
  display: flex;
  align-items: center;
}

.tree-content:hover {
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
