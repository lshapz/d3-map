<template>
  <div id="app">
    <Button :method="loadTree" name="Toggle Tree Display" />

  <div v-if="treeShow">
    <Tree :treeData="treeData" v-on:childToParent="setUpdateForm"/>
    <Button :method="addNode" name="Add a Node" />
    <Button :method="removeNode" name="Remove a Node" />
  <UpdateForm v-if="midChange" :savedText="currentText" v-on:childToParent="changeText"></UpdateForm>

  </div>
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
            { "name": 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce volutpat ante velit, sed feugiat nibh pulvinar nec.' },
            { "name": "Nullam et libero ac urna varius elementum. Vestibulum sed nibh ornare augue scelerisque venenatis in eu leo. Mauris aliquet tincidunt leo, hendrerit congue dui feugiat sed. Curabitur elementum dui et odio maximus pharetra." },
            { "name": "Nunc eget aliquet purus. Aenean et mattis sem."}
          ]
        },
        { "name": "Level 2: B" },
        {
          "name": "Level 2: C", 
          "children": [
            {"name": "Level 3: C",
              "children": [
                {"name": "Vivamus eleifend eu neque eu semper. Nullam malesuada ."}
              ] 
            },
            { "name": "Integer vehicula tincidunt massa ac aliquam. Sed leo enim, tincidunt et dictum sed, porta eget elit. Aenean ac sem at urna porta ornare vel mollis augue. ", 
              "children": [
                {"name": "Curabitur dapibus ex posuere turpis blandit, sed pulvinar metus dapibus. Nullam tellus sem, varius quis venenatis sed, aliquet id lectus. Duis tempor, est id pulvinar pretium, ."}
              ]
            }
          ] 
        },
        {"name": "Level 2: D",
        "children": [
          {"name": "Level 3: D", "children": [
            {"name": "Vivamus suscipit quam vitae justo pulvinar, nec maximus lectus sagittis. Phasellus vitae dui eget nibh venenatis cursus in vitae libero. Ut sed malesuada mauris. Curabitur id odio id sem sagittis pretium at sagittis dui."},
             {"name": "Duis quis massa ultricies, eleifend est id, mollis nisl. Nulla facilisi. Pellentesque aliquet, tortor a tincidunt pellentesque, ex diam commodo dolor, a pulvinar mauris augue at libero. Vestibulum orci nunc, luctus vitae felis vel", 
             "children": [{"name": "Level 4: D"}, {"name": "In in lectus vel dolor convallis dictum. Nunc quis consequat tortor. Vivamus semper nisl justo, in vulputate enim tempus bibendum. Ut non interdum enim, ac porttitor orci."}]
             }
            ]}

        ]}
      ]
    },
    midChange: false,
    currentText: "",
    parentName: "",
    treeShow: true
    }
  },
  methods: {
    loadTree() {
      this.treeShow = !this.treeShow;
    },
    addNode() {
      let foo = this.treeData;
      this.treeData = {};
      foo.children.push({"name": "Level 2: D"});
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
