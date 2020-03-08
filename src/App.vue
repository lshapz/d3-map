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
            { "name": 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce volutpat ante velit, sed feugiat nibh pulvinar nec. Vivamus suscipit quam vitae justo pulvinar, nec maximus lectus sagittis. Phasellus vitae dui eget nibh venenatis cursus in vitae libero. Ut sed malesuada mauris. Curabitur id odio id sem sagittis pretium at sagittis dui. Duis quis massa ultricies, eleifend est id, mollis nisl. Nulla facilisi. Pellentesque aliquet, tortor a tincidunt pellentesque, ex diam commodo dolor, a pulvinar mauris augue at libero. Vestibulum orci nunc, luctus vitae felis vel, porttitor dignissim ante. In in lectus vel dolor convallis dictum. Nunc quis consequat tortor. Vivamus semper nisl justo, in vulputate enim tempus bibendum. Ut non interdum enim, ac porttitor orci. Nunc eget aliquet purus. Aenean et mattis sem.' },
            { "name": "Nullam et libero ac urna varius elementum. Vestibulum sed nibh ornare augue scelerisque venenatis in eu leo. Mauris aliquet tincidunt leo, hendrerit congue dui feugiat sed. Curabitur elementum dui et odio maximus pharetra. Nam ultrices sit amet dolor vel sagittis. Praesent tincidunt erat a venenatis feugiat. Etiam ac eleifend tellus, tempor tempor lorem. Nam sollicitudin vitae massa id rutrum. Praesent mollis, arcu nec dignissim viverra, leo ipsum molestie odio, et gravida erat ipsum ut est. Maecenas gravida quis lectus vel aliquam. Nulla porttitor odio ac ligula euismod euismod eget sed massa." }
          ]
        },
        { "name": "Level 2: B" },
        {
          "name": "Level 2: C", 
          "children": [
            {"name": "Level 3: C",
              "children": [
                {"name": "Vivamus eleifend eu neque eu semper. Nullam malesuada semper mauris. Suspendisse vitae semper elit, at scelerisque orci. Vivamus eget ligula posuere, sagittis turpis id, tempus enim. Integer vel odio commodo, dignissim urna et, sodales nibh. Sed dictum ante id est laoreet congue. Curabitur pharetra ac massa vel dapibus. Suspendisse mollis ipsum ut mauris feugiat pharetra. Cras sed ligula aliquam, sollicitudin est venenatis, porttitor ipsum. Cras tempus id dui nec sodales. Quisque sed scelerisque ante. Integer pretium enim eget feugiat ultrices. Proin quis nisl sed mi semper fermentum sollicitudin in ipsum. Vivamus et justo ac nunc lobortis consectetur. Morbi id est id mauris sagittis aliquet ut sit amet sem."}
              ] 
            },
            { "name": "Integer vehicula tincidunt massa ac aliquam. Sed leo enim, tincidunt et dictum sed, porta eget elit. Aenean ac sem at urna porta ornare vel mollis augue. Nam eu tellus et felis congue sollicitudin at commodo tortor. Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Duis sed nulla sed metus accumsan sollicitudin. Nam felis felis, consectetur gravida molestie eget, varius vel elit. Praesent ornare nunc vitae orci mattis, in fermentum nunc commodo. Mauris orci leo, tincidunt at laoreet in, tempor nec est. Proin nec urna fringilla, interdum diam a, consectetur est. Phasellus ullamcorper nulla eget eros euismod, vel porttitor orci ornare. Nam porttitor sodales sem, at venenatis metus porta ornare. Cras vitae augue ut leo mollis aliquam et sit amet nisi.", 
              "children": [
                {"name": "Curabitur dapibus ex posuere turpis blandit, sed pulvinar metus dapibus. Nullam tellus sem, varius quis venenatis sed, aliquet id lectus. Duis tempor, est id pulvinar pretium, urna lectus tempor lorem, sed commodo tortor tellus eu nibh. Quisque justo urna, lacinia ut malesuada quis, convallis non lacus. Suspendisse purus eros, sollicitudin vel mattis id, suscipit in est. Vivamus aliquam, nunc vitae commodo fringilla, felis risus consectetur justo, a volutpat odio sem hendrerit metus. Nulla porta felis at volutpat varius. Suspendisse id turpis aliquam, convallis quam a, vestibulum libero. Pellentesque a erat porttitor, convallis erat nec, ornare magna."}
              ]
            }
          ] 
        }
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
