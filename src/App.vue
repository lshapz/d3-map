<template>
  <div id="app">
    <Tree :treeData="treeData" />
    <Button :method="addNode" name="Add a Node" />
    <Button :method="removeNode" name="Remove a Node" />

  </div>
</template>

<script>
import Tree from './components/Tree.vue'
import Button from './components/Button.vue'

export default {
  name: 'app',
  components: {
    Tree,
    Button
  },
  data() {
    return {
    treeData:   {
      "name": "Top Level",
      "children": [
        { 
          "name": "Level 2: A",
          "children": [
            { "name": "Son of A" },
            { "name": "Daughter of A" }
          ]
        },
        { "name": "Level 2: B" }
      ]
    }
    }
  },
  methods: {
    addNode() {
      let foo = this.treeData;
      this.treeData = {}
      foo.children.push({"name": "Level 3: C"})
      this.treeData = foo;
    },
    removeNode() {
      let foo = this.treeData;
      this.treeData = {}

      foo.children.splice(foo.children.length - 1, 1)
      this.treeData = foo;
    }
  },
  watch: {
    treeData(newdata, olddata) {
      debugger
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
