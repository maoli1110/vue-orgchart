<template>
    <div class="default-recursion">
        <div v-if="datasource.type!=='route'" class="node-wrap">
            <div class="node-wrap-box node_sid-startevent start-node">
                <div>
                    <div class="title" style="background: rgb(87, 106, 149);"><span class="">{{datasource.name}}</span></div>
                    <div class="content">
                        <div class="text" data-spm-anchor-id="0.0.0.i3.31bc4490aDuVls">毛小喵q99999</div><i class="anticon anticon-right arrow"></i>
                    </div>
                </div>
            </div>
            <div class="add-node-btn-box">
                <div class="add-node-btn"><button class="btn" type="button"><span class="iconfont" data-spm-anchor-id="0.0.0.i10.31bc4490aDuVls"></span></button></div>
            </div>
        </div>
        <template v-if="datasource.type==='route'">
            <div class="branch-wrap">
                <div class="branch-box-wrap">
                    <div class="branch-box"><button class="add-branch">添加条件</button>
                        <div class="col-box" v-for="(child, index) in datasource.conditionNodes" :key="index">
                            <div class="top-left-cover-line" v-if="index===0"></div>
                            <div class="bottom-left-cover-line" v-if="index===0"></div>
                            <div class="top-right-cover-line" v-if="index===datasource.conditionNodes.length-1"></div>
                            <div class="bottom-right-cover-line" v-if="index===datasource.conditionNodes.length-1"></div>
                            <div class="condition-node">
                                <div class="condition-node-box">
                                    <div class="auto-judge node_f115_cdf3">
                                        <div class="title-wrapper"><span class="editable-title">{{child.name}}</span><span class="priority-title">优先级1</span><i class="anticon anticon-close close"></i></div>
                                        <div class="content">
                                            <div>{{index===datasource.conditionNodes.length-1}}</div>
                                        </div>
                                        <div class="sort-right">&gt;</div>
                                    </div>
                                    <div @click="addCondition(child)" class="add-node-btn"><button class="btn" type="button"><span class="iconfont"></span></button></div>
                                </div>
                            </div>
                            <template v-if="child.childNode">
                                <node :datasource="child.childNode" :handle-click="handleClick">
                                    <template v-for="slot in Object.keys($scopedSlots)" :slot="slot" slot-scope="scope">
                                        <slot :name="slot" v-bind="scope" />
                                    </template>
                                </node>
                            </template>
                        </div>
                    </div>
                    <div class="add-node-btn-box">
                        <div class="add-node-btn"><button class="btn" type="button"><span class="iconfont"></span></button></div>
                    </div>
                </div>
            </div>
        </template>
        <template v-if="datasource.childNode">
            <!-- <div v-for="child in datasource.childNode" :key="child.nodeId" class="node-wrap"> -->
            <node :datasource="datasource.childNode" :handle-click="handleClick">
                <template v-for="slot in Object.keys($scopedSlots)" :slot="slot" slot-scope="scope">
                    <slot :name="slot" v-bind="scope" />
                </template>
            </node>
            <!-- </div> -->
        </template>
        <!--  <div class="end-node">
                <div class="end-node-circle"></div>
                <div class="end-node-text">流程结束{{datasource.childNode.length}}</div>
            </div> -->
    </div>
</template>
<script>
export default {
    name: 'node',
    props: {
        datasource: Object,
        handleClick: Function
    },
    methods: {
        addCondition(child) {
            console.log(child)
        }
    },
    mounted() {

    }

};
</script>
<style>
</style>