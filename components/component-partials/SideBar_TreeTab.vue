<script>
import TreeHealth from "./SideBar_TreeHealth.vue";
import TreeInfo from "./SideBar_TreeInfo.vue";
import TreeSurroundingArea from "./SideBar_TreeSurroundingArea.vue";

export default{
    Name:'TreeTab',
    components: {TreeHealth, TreeInfo, TreeSurroundingArea}
}
</script>
 
<template>
   <div>
       <TreeHealth/>
       <TreeInfo/>
       <TreeSurroundingArea/>
   </div>
</template>