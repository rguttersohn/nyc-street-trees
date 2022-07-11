<script>
import CDData from "./SideBar_CDData.vue"
export default {
    Name:'CDTab',
    components:{CDData},
    setup(props){
        const {tab} = props

        return {tab}
    }
}
</script>

<template>
    <div>
        <CDData/>
    </div>
</template>