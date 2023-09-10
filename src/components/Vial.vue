<template lang="pug">
//- div(style="display: flex; flex-direction: column; width: 8rem;")
//-     div(v-for="item in vialContent" style="width: 100%; height: 5rem;" :style="{'background-color': item}")
svg(xmlns="http://www.w3.org/2000/svg"
    version="1.1"
    viewBox="0 0 100 120"
    preserveAspectRatio="xMidYMid slice"
    style="width: 5rem; height:15rem; border-bottom-left-radius: 25%; border-bottom-right-radius: 25%;")
    rect(v-for="(item, index) in vialContent" x="0" :y="105-index*15" width="100%" height="15" :style="{'fill': item}")
</template>

<script>

export default {
    props: ['vialContent'],
    computed: {
        full: function () {
            return this.vialContent.length === 8
        },
        empty: function () {
            return this.vialContent.length
        }
    }
}

</script>