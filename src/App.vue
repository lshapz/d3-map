<template>
  <div id="app">
    <Tree :treeData="treeData" v-on:childToParent="setUpdateForm"/>
    <Button :method="addNode" name="Add a Node" />
    <Button :method="removeNode" name="Remove a Node" />
  <UpdateForm v-if="midChange" :savedText="currentText" v-on:childToParent="changeText"></UpdateForm>
  </div>
</template>

<script>
import Tree from './components/Tree.vue'
import Button from './components/Button.vue'
import UpdateForm from './components/UpdateForm.vue'
import * as _ from 'lodash';

export default {
  name: 'app',
  components: {
    Tree,
    Button,
    UpdateForm
  },
  data() {
    return {
    treeData:  {
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
    },
    midChange: false,
    currentText: "",
    parentName: ""
    }
  },
  methods: {
    addNode() {
      let foo = this.treeData;
      this.treeData = {};
      foo.children.push({"name": "Level 3: C"});
      this.treeData = foo;
    },
    removeNode() {
      let foo = this.treeData;
      this.treeData = {};

      foo.children.splice(foo.children.length - 1, 1)
      this.treeData = foo;
    },
    changeText(text) {
      let index = this.peekThroughChildren(this.treeData, this.currentText, text)
      // let thing = _.find(this.treeData, {name: this.currentText});
      if (this.parentName !== "Top Level") {
        let foo = _.findIndex(this.treeData.children, {"name": this.parentName});
        debugger
        this.treeData.children[foo].children[index].name = text
      } else {
        this.treeData.children[index].name = text
      }
      this.midChange = false;
      this.currentText = "";
      this.parentName = "";

    },
    peekThroughChildren(obj, string, newString) {
      // debugger
      if (obj["children"]) {
        let index = _.findIndex(obj["children"], {"name": string})
        if (index !== -1) {
          this.parentName = obj["name"]
          return index
        } else {
          let thing;
          obj["children"].forEach(item=>{
            if (item["children"]) {
              this.parentName = item["name"]
              thing =  this.peekThroughChildren(item, string, newString)
            }
          })
          return thing;

          // index = this.peekThroughChildren(, string, newString)

        }
        // debugger
      }
    },
    setUpdateForm(foobar) {
      this.midChange = true;
      this.currentText = foobar;
    }
  },
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
