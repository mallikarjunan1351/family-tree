<template>
  <div>
    <b-button class="m-2" @click="addNode" variant="success"
      >Add First Person</b-button
    >
    <b-button class="m-2" @click="addFatherNode" variant="success"
      >Add Father Node</b-button
    >
    <b-button class="m-2" @click="addMotherNode" variant="success"
      >Add Mother Node</b-button
    >
    <vue-tree-list
      @click="onClick"
      @change-name="onChangeName"
      @delete-node="onDel"
      @add-node="onAddNode"
      :model="data"
      default-tree-node-name="new node"
      default-leaf-node-name="new leaf"
      v-bind:default-expanded="false"
    >
      <template v-slot:leafNameDisplay="slotProps">
        <span>
          {{ slotProps.model.name }}
          <span class="muted">#{{ slotProps.model.id }}</span>
        </span>
      </template>
      <span class="icon" slot="addTreeNodeIcon">📂</span>
      <span class="icon" slot="addLeafNodeIcon">＋</span>
      <span class="icon" slot="editNodeIcon">📃</span>
      <span class="icon" slot="delNodeIcon">✂️</span>
      <span class="icon" slot="leafNodeIcon">🍃</span>
      <span class="icon" slot="treeNodeIcon">🌲</span>
    </vue-tree-list>
    <b-button class="m-2" @click="getNewTree" variant="info"
      >Get new tree</b-button
    >
    <pre>
      {{ newTree }}
    </pre>
  </div>
</template>

<script>
import { VueTreeList, Tree, TreeNode } from "vue-tree-list";
export default {
  components: {
    VueTreeList,
  },
  data() {
    return {
      newTree: {},
      data: new Tree([]),
    };
  },
  methods: {
    onDel(node) {
      console.log(node);
      node.remove();
    },

    onChangeName(params) {
      console.log(params);
    },

    onAddNode(params) {
      console.log(params);
    },

    onClick(params) {
      console.log(params);
    },

    addFatherNode() {
      var node = new TreeNode({
        firstPerson: {
          name: "Katia",
          image: "https://picsum.photos/300/300?random=3",
          age: 23,
          relationship: "Father",
          gender: "male",
        },
        name: "father node",
        isLeaf: false,
      });
      if (!this.data.children) this.data.children = [];
      this.data.addChildren(node);
    },

    addMotherNode() {
      var node = new TreeNode({
        secondPerson: {
          name: "Oleg",
          image: "https://picsum.photos/300/300?random=3",
          age: "23",
          relationship: "Mother",
          gender: "female",
        },
        name: "mother node",
        isLeaf: false,
      });
      if (!this.data.children) this.data.children = [];
      this.data.addChildren(node);
    },

    addNode() {
      var node = new TreeNode({
        firstPerson: {
          name: "Katia",
          image: "https://picsum.photos/300/300?random=3",
          age: "23",
          relationship: "Father",
          gender: "male",
        },
        name: "new node",
        isLeaf: false,
      });
      if (!this.data.children) this.data.children = [];
      this.data.addChildren(node);
    },

    getNewTree() {
      var vm = this;
      function _dfs(oldNode) {
        var newNode = {};

        for (var k in oldNode) {
          if (k !== "children" && k !== "parent") {
            newNode[k] = oldNode[k];
          }
        }

        if (oldNode.children && oldNode.children.length > 0) {
          newNode.children = [];
          for (var i = 0, len = oldNode.children.length; i < len; i++) {
            newNode.children.push(_dfs(oldNode.children[i]));
          }
        }
        return newNode;
      }

      vm.newTree = _dfs(vm.data);
      this.$emit("treelist", vm.newTree);
    },
  },
};
</script>

<style lang="sass" rel="stylesheet/sass">
.vtl

  &-drag-disabled
    background-color: #d0cfcf
    &:hover
      background-color: #d0cfcf

    &-disabled
      background-color: #d0cfcf
</style>

<style lang="sass" rel="stylesheet/sass" scoped>
.icon
  &:hover
    cursor: pointer

.muted
  color: gray
  font-size: 80%
</style>