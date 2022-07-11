<script>
import {useStore} from 'vuex';
import { onMounted, computed, watch, reactive} from 'vue';
import {makeChart} from '../../controller/ChartRender';
import vault from '../../../vault.js'


    export default {
        Name: 'CDData',
        setup(){
            const apiToken = vault.apiToken;
            const store = useStore();
            const activeCommunityDistrict = computed(()=>store.state.activeCommunityDistrict)
            const trees_all = reactive([
                {
                    name: 'Trees Counted',
                    data: []
                }
            ])


            const getCDTrees_all =  (activeCommunityDistrict)=>{
                const seriesData = [];
                fetch(`https://data.cityofnewyork.us/resource/kyad-zm4j.json?$$app_token=${apiToken}&cb_new=${activeCommunityDistrict}&$select=count(cb_original)`)
                    .then(resp => resp.json())
                    .then(data => seriesData.push(data[0]["count_cb_original"]))
                fetch(`https://data.cityofnewyork.us/resource/29bw-z7pj.json?$$app_token=${apiToken}&cb_num=${activeCommunityDistrict}&$select=count(cb_num)`)
                    .then(response => response.json())
                    .then(data => seriesData.push(data[0]["count_cb_num"]))
                fetch(`https://data.cityofnewyork.us/resource/uvpi-gqnh.json?$$app_token=${apiToken}&cb_num=${activeCommunityDistrict}&status=Alive&$select=count(cb_num)`)
                    .then(response => response.json())
                    .then(data => seriesData.push(data[0]["count_cb_num"]))
                trees_all[0].data = seriesData;
                makeChart(document.querySelector('#chart_trees-all'), trees_all, 'line', ['green']);     
            }

            onMounted(()=>{
                getCDTrees_all(activeCommunityDistrict.value);
            })

            watch(activeCommunityDistrict, ()=> {
                getCDTrees_all(activeCommunityDistrict.value)
            })

           
            
            return { activeCommunityDistrict }
        }
    }
</script>

<template> 
   <div>
        <h3 class="font-bold">Trees counted in Community District {{activeCommunityDistrict}} in 1995, 2005, and 2015</h3>
        <div id="chart_trees-all"></div>
   </div>
</template>