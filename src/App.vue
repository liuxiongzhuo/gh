<script setup>
import { computed } from 'vue';
import Owner from './Owner.vue';
import Four04 from './Four04.vue';
import Repo from './Repo.vue';
import Tree from './Tree.vue';
import Releases from './Releases.vue';
import Tag from './Tag.vue';
import Branches from './Branches.vue';
import Home from './Home.vue'
let path_array = window.location.pathname.split('/')
if (path_array[0]=='') {
  path_array.shift()
}
if (path_array[path_array.length-1]=='') {
  path_array.pop()
}
const currentView=computed(()=>{
  if (path_array.length==0) {
    return Home
  }else if (path_array.length==1&&path_array[0]!=404) {
    return Owner
  }else if (path_array.length==1&&path_array[0]==404){
    return Four04
  }else if (path_array.length==2){
    return Repo
  }else if (path_array.length >= 4 && path_array[2] == 'tree'){
    return Tree
  }else if (path_array.length==3 && path_array[2] == 'releases'){
    return Releases
  }else if (path_array.length==3 && path_array[2] == 'branches'){
    return Branches
  }else if (path_array.length==5 && path_array[2] == 'releases'&& path_array[3] == 'tag'){
    return Tag
  }

})
</script>

<template>
  <component :is="currentView" :owner="path_array[0]" :repo="path_array[1]" :branch="path_array[3]"></component>
</template>

<style scoped>

</style>
