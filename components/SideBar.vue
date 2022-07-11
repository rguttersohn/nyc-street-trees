
<script>
import { useStore } from 'vuex';
import { computed, ref } from 'vue';
import TreeTab from './component-partials/SideBar_TreeTab.vue';
import CDTab from './component-partials/SideBar_CDTab.vue';


export default {
  components:{
    TreeTab,
    CDTab
    },
  setup(){
    const store = useStore();
    const activeTree = computed(()=>store.state.activeTree);
    const sideBarActive = computed(()=>store.state.sideBarActive);
    const activeTab = computed(()=>store.state.activeTab)
    const closeSideBar = ()=> store.commit('setSideBarFalse')
    const setActiveTab = (event) => {
          store.commit('setActiveTab', event.target.dataset.tabName)
    }
    return { sideBarActive, activeTree, closeSideBar, activeTab, setActiveTab, TreeTab, CDTab}
  }
};
</script>
<template>
  <div
    :class="{'w-1/4': sideBarActive, 'w-0': !sideBarActive}"
    class="transition-width duration-300 z-10 p-3"
  >
    <div v-if="sideBarActive">
      <button @click="closeSideBar" class="block mx-auto mb-10 px-3 py-1 cursor-pointer border border-light-100 rounded-lg">Close Sidebar</button>
      <div class="flex justify-evenly mb-10">
      <h3 
        :class="{'hover:underline cursor-pointer' : activeTab !== 'cd', 'underline' : activeTab === 'cd'}"
        @click="setActiveTab"
        data-tab-name="cd"
        >Community District</h3>
      <h3
        :class="{'hover:underline cursor-pointer' : activeTab !== 'tree', 'underline' : activeTab === 'tree'}"
        @click="setActiveTab"
        data-tab-name="tree"
        >Tree</h3>
    </div>
    <div>
      <KeepAlive>
        <component :is="activeTab === 'cd' ? CDTab : TreeTab" ></component>
      </KeepAlive>
    </div>
  </div>
  </div>
  
</template>