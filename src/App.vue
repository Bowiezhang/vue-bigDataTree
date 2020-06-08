<template>
  <div id="app">
    <div class="content">
      <el-tree
        :props="props"
        :load="loadNode"
        lazy
        show-checkbox>
      </el-tree>
    </div>
    <div class="content">
      <big-data-tree></big-data-tree>
    </div>
  </div>
</template>

<script>
import BigDataTree from '@/components/big-data-tree.vue'

export default {
  name: 'App',

  components: { BigDataTree },

  data() {
    return {
      props: {
        label: 'name',
        children: 'children',
        disabled: 'disabled',
        isLeaf: 'leaf'
      },
    }
  },

  methods: {
    loadNode(node, resolve) {
      console.log(resolve)
      const list = []
      for (let index = 0; index < 30; index++) {
        list[index] = { name: `节点${index}` }
      }
      
      return resolve(list)
    }
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  position: absolute;
  top: 0;
  left: 0;
  height: calc(100% - 100px);
  width: calc(100% - 100px);
  padding: 50px;
  display: flex;
  justify-content: space-between;
}

.content {
  width: 30%;
  height: 100%;
  overflow: auto;
}
</style>
