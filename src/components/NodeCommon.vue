<template>
    <div>
        <!-- <div class="box-scale" id="box-scale" style="transform: scale(1); transform-origin: 50% 0px 0px;" data-spm-anchor-id="0.0.0.i9.31bc4490aDuVls"> -->
            <!-- childNode.type=="approver" -->
            <!-- childNode.type=="route" -->
            <!-- childNode.type=="route" -->
            <div class="node-wrap" data-spm-anchor-id="0.0.0.i6.31bc4490aDuVls">
                <div class="node-wrap-box node_sid-startevent start-node">
                    <div>
                        <div class="title" style="background: rgb(87, 106, 149);"><span class="">{{datasource.name}}</span></div>
                        <div class="content">
                            <div class="text" data-spm-anchor-id="0.0.0.i3.31bc4490aDuVls">毛小喵q</div><i class="anticon anticon-right arrow"></i>
                        </div>
                    </div>
                </div>
                <div class="add-node-btn-box">
                    <div class="add-node-btn"><button class="btn" type="button"><span class="iconfont" data-spm-anchor-id="0.0.0.i10.31bc4490aDuVls"></span></button></div>
                </div>
            </div>
           <!--  <div class="end-node">
                <div class="end-node-circle"></div>
                <div class="end-node-text">流程结束{{datasource.childNode.length}}</div>
            </div> -->
        </div>
</template>
<script>
export default {
    name: 'node-common',
    props: {
        datasource: Object,
        handleClick: Function
    },
    methods: {},
    mounted() {
        setTimeout(() => {
            console.log(Object.keys(this.$scopedSlots), '$scopedSlots')
        }, 100)

    }

};
</script>
<style>
</style>