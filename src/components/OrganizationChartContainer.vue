<template>
  <div
    v-bind="{ scopedSlots: $scopedSlots }"
    class=""
    @wheel="zoom && zoomHandler($event)"
    @mouseup="pan && panning && panEndHandler($event)"
  >
    <div
      class="dingflow-design"
      :style="{ transform: transformVal, cursor: cursorVal }"
      @mousedown="pan && panStartHandler($event)"
      @mousemove="pan && panning && panHandler($event)"
    >
      <div
        class="box-scale"
        id="boex-scale"
        style="transform-origin: 50% 0px 0px; transform: scale(1);"
      >
        <organization-chart-node
          :datasource="datasource"
          :handle-click="handleClick"
        >
          <template
            v-for="slot in Object.keys($scopedSlots)"
            :slot="slot"
            slot-scope="scope"
          >
            <slot
              :name="slot"
              v-bind="scope"
            />
          </template>
</organization-chart-node>
</div>
</div>
</div>
</template>

<script>
    import $ from "jquery";
    import OrganizationChartNode from "./OrganizationChartNode.vue";
    // import OrganizationChartNode from './OrganizationChartNode.vue'
    import "../../static/css/design.css";
    /**
     * 线性数据转化为树
     * @param {Object} data 源数据
     * @param {Object} parentKey 父级id key
     * @param {childrenKey} childrenKey 子集key
     * @param {Object} pId 父级标识符
     */
    function toTree(data, parentKey, childrenKey, pId) {
        var tree = [];
        var temp;
        for (let i = 0; i < data.length; i++) {
            if (data[i][parentKey] == pId) {
                var obj = data[i];
                temp = toTree(data, parentKey, childrenKey, data[i][childrenKey]); // 递归获取当前节点的子节点
                // console.log(temp, "temp");
                if (temp.length > 0) {
                    let lineArray = [];
                    let nodeArray = {};
                    for (let j = 0; j < temp.length; j++) {
                        if (temp[j].type == "line") {
                            lineArray.push(temp[j]);
                        } else {
                            nodeArray = temp[j];
                        }
                    }
                    if (lineArray.length > 0) {
                        obj.conditions = lineArray; // 分支条件节点
                    }
                    if (nodeArray.nodeId) {
                        obj.children = nodeArray; // 审批人节点&发起人节点
                    }
                }

                tree.push(obj);
            }
        }
        return tree;
    }

    /**
     * 处理 JSON 嵌套格式的数据转换为简单 Array 格式
     * 1.分别获取nodeArr & lineArr
     */
    function transformToArray(treeList) {
        let currentNode = treeList;
        console.log(treeList,'nodeArrayList111 ')
        let nodeObj = {};
        let lineObj = {};
        // if(currentNode.type){ // 非分支节点
            nodeObj.nodeId = currentNode.nodeId;
            nodeObj.nodeName = currentNode.nodeName;
            nodeObj.type = currentNode.type;
            nodeArrayList.push(nodeObj);
        // }
        if(currentNode.type==='JOIN_NODE'){ // 分支节点
                currentNode.conditions.forEach(item => {
                    lineObj.nodeId = currentNode.nodeId;
                    lineObj.nodeName = currentNode.nodeName;
                    lineObj.type = currentNode.type;
                    lineArrayList.push(lineObj);
                    if(item.children){
                        transformToArray(item.children)
                    }
                });
        }
        if(currentNode.children){
            transformToArray(currentNode.children)
        }
    }

    function flatten (arr) {
        console.log(arr,'arr')
        return arr.reduce((arr, {nodeId, nodeName, prevNodeId, children = []}) =>
        arr.concat([{nodeId, nodeName, prevNodeId}], flatten(children)), [])
    }
    let nodeArrayList = [];
    let lineArrayList = [];
    export default {
        name: "Container",
        props: {
            datasource: {
                type: Object,
                required: true
            },
            pan: {
                type: Boolean,
                required: false
            },
            zoom: {
                type: Boolean,
                required: false
            },
            zoomoutLimit: {
                type: Number,
                required: false,
                default: 0.5
            },
            zoominLimit: {
                type: Number,
                required: false,
                default: 7
            }
        },
        data() {
            return {
                cursorVal: "default",
                panning: false,
                startX: 0,
                startY: 0,
                transformVal: ""
            };
        },
        components: {
            OrganizationChartNode
        },
        methods: {
            handleClick(nodeData) {
                this.$emit("node-click", nodeData);
            },
            panEndHandler() {
                this.panning = false;
                this.cursorVal = "default";
            },
            panHandler(e) {
                let newX = 0;
                let newY = 0;
                if (!e.targetTouches) {
                    // pand on desktop
                    newX = e.pageX - this.startX;
                    newY = e.pageY - this.startY;
                } else if (e.targetTouches.length === 1) {
                    // pan on mobile device
                    newX = e.targetTouches[0].pageX - this.startX;
                    newY = e.targetTouches[0].pageY - this.startY;
                } else if (e.targetTouches.length > 1) {
                    return;
                }
                if (this.transformVal === "") {
                    if (this.transformVal.indexOf("3d") === -1) {
                        this.transformVal = "matrix(1,0,0,1," + newX + "," + newY + ")";
                    } else {
                        this.transformVal =
                            "matrix3d(1,0,0,0,0,1,0,0,0,0,1,0," + newX + ", " + newY + ",0,1)";
                    }
                } else {
                    let matrix = this.transformVal.split(",");
                    if (this.transformVal.indexOf("3d") === -1) {
                        matrix[4] = newX;
                        matrix[5] = newY + ")";
                    } else {
                        matrix[12] = newX;
                        matrix[13] = newY;
                    }
                    this.transformVal = matrix.join(",");
                }
            },
            panStartHandler(e) {
                if ($(e.target).closest(".node").length) {
                    this.panning = false;
                    return;
                } else {
                    this.cursorVal = "move";
                    this.panning = true;
                }
                let lastX = 0;
                let lastY = 0;
                if (this.transformVal !== "") {
                    let matrix = this.transformVal.split(",");
                    if (this.transformVal.indexOf("3d") === -1) {
                        lastX = parseInt(matrix[4]);
                        lastY = parseInt(matrix[5]);
                    } else {
                        lastX = parseInt(matrix[12]);
                        lastY = parseInt(matrix[13]);
                    }
                }
                if (!e.targetTouches) {
                    // pand on desktop
                    this.startX = e.pageX - lastX;
                    this.startY = e.pageY - lastY;
                } else if (e.targetTouches.length === 1) {
                    // pan on mobile device
                    this.startX = e.targetTouches[0].pageX - lastX;
                    this.startY = e.targetTouches[0].pageY - lastY;
                } else if (e.targetTouches.length > 1) {
                    return;
                }
            },
            setChartScale(newScale) {
                let matrix = "";
                let targetScale = 1;
                if (this.transformVal === "") {
                    this.transformVal =
                        "matrix(" + newScale + ", 0, 0, " + newScale + ", 0, 0)";
                } else {
                    matrix = this.transformVal.split(",");
                    if (this.transformVal.indexOf("3d") === -1) {
                        targetScale = Math.abs(window.parseFloat(matrix[3]) * newScale);
                        if (
                            targetScale > this.zoomoutLimit &&
                            targetScale < this.zoominLimit
                        ) {
                            matrix[0] = "matrix(" + targetScale;
                            matrix[3] = targetScale;
                            this.transformVal = matrix.join(",");
                        }
                    } else {
                        targetScale = Math.abs(window.parseFloat(matrix[5]) * newScale);
                        if (
                            targetScale > this.zoomoutLimit &&
                            targetScale < this.zoominLimit
                        ) {
                            matrix[0] = "matrix3d(" + targetScale;
                            matrix[5] = targetScale;
                            this.transformVal = matrix.join(",");
                        }
                    }
                }
            },
            zoomHandler(e) {
                let newScale = 1 + (e.deltaY > 0 ? -0.2 : 0.2);
                this.setChartScale(newScale);
            },
            /**
             * 处理接口数据为前端可用的类树结构
             * 1.遍历Node节点，找出其对应的父节点
             * 2.遍历line节点，虚拟出JOIN_NODE下的conditionNodes
             */
            transformToTreeNodes() {
                var originList = {
                    processNodeLineList: [{
                            lineId: "line1",
                            prevNodeId: "1",
                            nextNodeId: "2"
                        },
                        {
                            lineId: "line2",
                            prevNodeId: "2",
                            nextNodeId: "3"
                        },

                        {
                            lineId: "line3",
                            prevNodeId: "3",
                            nextNodeId: "4"
                        },
                        {
                            lineId: "line4",
                            prevNodeId: "3",
                            nextNodeId: "8"
                        },
                        {
                            lineId: "line5",
                            prevNodeId: "3",
                            nextNodeId: "8"
                        },

                        {
                            lineId: "line6",
                            prevNodeId: "4",
                            nextNodeId: "5"
                        },

                        {
                            lineId: "line7",
                            prevNodeId: "5",
                            nextNodeId: "6"
                        },
                        {
                            lineId: "line8",
                            prevNodeId: "5",
                            nextNodeId: "6"
                        },
                        {
                            lineId: "line9",
                            prevNodeId: "6",
                            nextNodeId: "7"
                        },
                        {
                            lineId: "line10",
                            prevNodeId: "7",
                            nextNodeId: "8"
                        },
                        {
                            lineId: "line11",
                            prevNodeId: "7",
                            nextNodeId: "8"
                        },
                        {
                            lineId: "line12",
                            prevNodeId: "8",
                            nextNodeId: "9"
                        },
                        {
                            lineId: "line13",
                            prevNodeId: "9",
                            nextNodeId: "10"
                        },
                        {
                            lineId: "line14",
                            prevNodeId: "9",
                            nextNodeId: "11"
                        }
                    ],
                    processNodeList: [{
                            nodeId: "1",
                            nodeName: "发起人",
                            type: "TASK_NODE",
                            prevNodeId: "0"
                        },
                        {
                            nodeId: "2",
                            nodeName: "毛小喵1",
                            type: "TASK_NODE"
                        },

                        {
                            nodeId: "3",
                            nodeName: "分支",
                            type: "JOIN_NODE"
                        },

                        {
                            nodeId: "4",
                            nodeName: "毛小喵2",
                            type: "TASK_NODE"
                        },

                        {
                            nodeId: "5",
                            nodeName: "分支",
                            type: "JOIN_NODE"
                        },

                        {
                            nodeId: "6",
                            nodeName: "毛小喵3",
                            type: "TASK_NODE"
                        },

                        {
                            nodeId: "7",
                            nodeName: "分支",
                            type: "JOIN_NODE"
                        },

                        {
                            nodeId: "8",
                            nodeName: "毛小喵4",
                            type: "TASK_NODE"
                        },

                        {
                            nodeId: "9",
                            nodeName: "分支",
                            type: "JOIN_NODE"
                        },

                        {
                            nodeId: "10",
                            nodeName: "毛小喵5",
                            type: "TASK_NODE"
                        },
                        {
                            nodeId: "11",
                            nodeName: "结束",
                            type: "END_NODE"
                        }
                    ]
                };
                let JoinNodeArr = [];
                // 获取分支节点集合
                originList.processNodeList.forEach(item => {
                    if (item.type === "JOIN_NODE") {
                        JoinNodeArr.push(item.nodeId);
                    }
                });
                //  console.log(JoinNodeArr, "joinNodeARR");
                /**
                 * 遍历Node节点，找出其对应的父节点
                 * 不同节点的父id不同(分三种情况，具体看下面代码)
                 */
                originList.processNodeList.forEach(function(item) {
                    originList.processNodeLineList.forEach(function(item1) {
                        // processNodeLineList里面的nextNodeId匹配到当前节点的nodeId
                        if (item.nodeId == item1.nextNodeId) {
                            item.prevNodeId = "";
                            // 判断是否是分支的终点（多个节点的nextNodeId是当前节点，说明当前节点是分支的终点）
                            let isRouteEndNode = false;
                            let prevRouteCount = originList.processNodeLineList.filter(
                                item2 => {
                                    return item2.nextNodeId == item.nodeId;
                                }
                            );
                            isRouteEndNode = prevRouteCount.length>1?true:false;
                            // console.log(prevRouteCount, "prevRouteCount");
                            // 当前节点是审批节点,分支的终点&当前节点的父节点是分支节点=>父级id则为当前匹配到的第一个节点的prevNodeId
                            // 当前节点是审批节点&当前节点的父节点是分支节点 => 父级id则为当前匹配到的lineId
                            // others
                            if (
                                item.type == "TASK_NODE" &&
                                isRouteEndNode &&
                                JoinNodeArr.indexOf(item1.prevNodeId) !== -1
                            ) {
                                item.prevNodeId = prevRouteCount[0].prevNodeId;
                            } else if (
                                item.type == "TASK_NODE" &&
                                !isRouteEndNode &&
                                JoinNodeArr.indexOf(item1.prevNodeId) !== -1
                            ) {
                                item.prevNodeId = item1.lineId;
                            } else {
                                item.prevNodeId = item1.prevNodeId;
                            }
                        }
                    });
                });
                /**
                 * 遍历line节点，虚拟出JOIN_NODE下的conditionNodes
                 * 凡是prevNode是JOIN_NODE,那么这条线就是虚拟节点
                 */
                let conditionNodes = originList.processNodeLineList.filter(item => {
                    return JoinNodeArr.indexOf(item.prevNodeId) !== -1;
                });
                conditionNodes = conditionNodes.map(item => {
                    return {
                        nodeId: item.lineId,
                        prevNodeId: item.prevNodeId,
                        nextNodeId: item.nextNodeId,
                        type: "line"
                    };
                });
                let combineNodeList = originList.processNodeList.concat(conditionNodes);

                return combineNodeList;

                //   console.log(conditionNodes, "conditionNodes");

                //   console.log(combineNodeList, "combineNodeList");
            },

        },
        mounted() {
            setTimeout(() => {
                let combineNodeList1 = this.transformToTreeNodes();
                let treeList = toTree(combineNodeList1, "prevNodeId", "nodeId", "0");
                console.log(treeList, "treeList1111");
                setTimeout(() => {
                    transformToArray(treeList[0]);
                    console.log(nodeArrayList,'nodeArrayList')
                    
                }, 100);
                // console.log(JSON.stringify(combineNodeList1), "combineNodeList1");
            }, 100);
        }
    };
</script>

<style>
    .orgchart-container {
        position: relative;
        display: inline-block;
        height: 100vh;
        width: calc(100% - 24px);
        border: 2px dashed #aaa;
        border-radius: 5px;
        overflow: auto;
        text-align: center;
    }

    .orgchart {
        box-sizing: border-box;
        display: inline-block;
        min-height: 202px;
        min-width: 202px;
        -webkit-touch-callout: none;
        -webkit-user-select: none;
        -khtml-user-select: none;
        -moz-user-select: none;
        -ms-user-select: none;
        user-select: none;
        background-image: linear-gradient( 90deg, rgba(200, 0, 0, 0.15) 10%, rgba(0, 0, 0, 0) 10%), linear-gradient(rgba(200, 0, 0, 0.15) 10%, rgba(0, 0, 0, 0) 10%);
        background-size: 10px 10px;
        border: 1px dashed rgba(0, 0, 0, 0);
        padding: 20px;
    }

    .orgchart .hidden,
    .orgchart~.hidden {
        display: none;
    }

    .orgchart.b2t {
        transform: rotate(180deg);
    }

    .orgchart.l2r {
        position: absolute;
        transform: rotate(-90deg) rotateY(180deg);
        transform-origin: left top;
    }

    .orgchart .verticalNodes ul {
        list-style: none;
        margin: 0;
        padding-left: 18px;
        text-align: left;
    }

    .orgchart .verticalNodes ul:first-child {
        margin-top: 2px;
    }

    .orgchart .verticalNodes>td::before {
        content: "";
        border: 1px solid rgba(217, 83, 79, 0.8);
    }

    .orgchart .verticalNodes>td>ul>li:first-child::before {
        box-sizing: border-box;
        top: -4px;
        height: 30px;
        width: calc(50% - 2px);
        border-width: 2px 0 0 2px;
    }

    .orgchart .verticalNodes ul>li {
        position: relative;
    }

    .orgchart .verticalNodes ul>li::before,
    .orgchart .verticalNodes ul>li::after {
        box-sizing: border-box;
        content: "";
        position: absolute;
        left: -6px;
        border-color: rgba(217, 83, 79, 0.8);
        border-style: solid;
        border-width: 0 0 2px 2px;
    }

    .orgchart .verticalNodes ul>li::before {
        top: -4px;
        height: 30px;
        width: 11px;
    }

    .orgchart .verticalNodes ul>li::after {
        top: 1px;
        height: 100%;
    }

    .orgchart .verticalNodes ul>li:first-child::after {
        box-sizing: border-box;
        top: 24px;
        width: 11px;
        border-width: 2px 0 0 2px;
    }

    .orgchart .verticalNodes ul>li:last-child::after {
        box-sizing: border-box;
        border-width: 2px 0 0;
    }

    .orgchart.r2l {
        position: absolute;
        transform: rotate(90deg);
        transform-origin: left top;
    }

    .orgchart>.spinner {
        font-size: 100px;
        margin-top: 30px;
        color: rgba(68, 157, 68, 0.8);
    }

    .orgchart table {
        border-spacing: 0;
        border-collapse: separate;
    }

    .orgchart>table:first-child {
        margin: 20px auto;
    }

    .orgchart td {
        text-align: center;
        vertical-align: top;
        padding: 0;
    }

    .orgchart .lines:nth-child(3) td {
        box-sizing: border-box;
        height: 20px;
    }

    .orgchart .lines .topLine {
        border-top: 2px solid rgba(217, 83, 79, 0.8);
    }

    .orgchart .lines .rightLine {
        border-right: 1px solid rgba(217, 83, 79, 0.8);
        float: none;
        border-radius: 0;
    }

    .orgchart .lines .leftLine {
        border-left: 1px solid rgba(217, 83, 79, 0.8);
        float: none;
        border-radius: 0;
    }

    .orgchart .lines .downLine {
        background-color: rgba(217, 83, 79, 0.8);
        margin: 0 auto;
        height: 20px;
        width: 2px;
        float: none;
    }

    /* node styling */

    .orgchart .node {
        box-sizing: border-box;
        display: inline-block;
        position: relative;
        margin: 0;
        padding: 3px;
        border: 2px dashed transparent;
        text-align: center;
        width: 130px;
    }

    .orgchart.l2r .node,
    .orgchart.r2l .node {
        width: 50px;
        height: 130px;
    }

    .orgchart .node>.spinner {
        position: absolute;
        top: calc(50% - 15px);
        left: calc(50% - 15px);
        vertical-align: middle;
        font-size: 30px;
        color: rgba(68, 157, 68, 0.8);
    }

    .orgchart .node:hover {
        background-color: rgba(238, 217, 54, 0.5);
        transition: 0.5s;
        cursor: default;
        z-index: 20;
    }

    .orgchart .node.focused {
        background-color: rgba(238, 217, 54, 0.5);
    }

    .orgchart .ghost-node {
        position: fixed;
        left: -10000px;
        top: -10000px;
    }

    .orgchart .ghost-node rect {
        fill: #ffffff;
        stroke: #bf0000;
    }

    .orgchart .node.allowedDrop {
        border-color: rgba(68, 157, 68, 0.9);
    }

    .orgchart .node .title {
        text-align: center;
        font-size: 12px;
        font-weight: bold;
        height: 20px;
        line-height: 20px;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
        background-color: rgba(217, 83, 79, 0.8);
        color: #fff;
        border-radius: 4px 4px 0 0;
    }

    .orgchart.b2t .node .title {
        transform: rotate(-180deg);
        transform-origin: center bottom;
    }

    .orgchart.l2r .node .title {
        transform: rotate(-90deg) translate(-40px, -40px) rotateY(180deg);
        transform-origin: bottom center;
        width: 120px;
    }

    .orgchart.r2l .node .title {
        transform: rotate(-90deg) translate(-40px, -40px);
        transform-origin: bottom center;
        width: 120px;
    }

    .orgchart .node .title .symbol {
        float: left;
        margin-top: 4px;
        margin-left: 2px;
    }

    .orgchart .node .content {
        box-sizing: border-box;
        width: 100%;
        height: 20px;
        font-size: 11px;
        line-height: 18px;
        border: 1px solid rgba(217, 83, 79, 0.8);
        border-radius: 0 0 4px 4px;
        text-align: center;
        background-color: #fff;
        color: #333;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
    }

    .orgchart.b2t .node .content {
        transform: rotate(180deg);
        transform-origin: center top;
    }

    .orgchart.l2r .node .content {
        transform: rotate(-90deg) translate(-40px, -40px) rotateY(180deg);
        transform-origin: top center;
        width: 120px;
    }

    .orgchart.r2l .node .content {
        transform: rotate(-90deg) translate(-40px, -40px);
        transform-origin: top center;
        width: 120px;
    }

    .orgchart .node .edge {
        font-size: 15px;
        position: absolute;
        color: rgba(68, 157, 68, 0.5);
        cursor: default;
        transition: 0.2s;
    }

    .orgchart.noncollapsable .node .edge {
        display: none;
    }

    .orgchart .edge:hover {
        color: #449d44;
        cursor: pointer;
    }

    .orgchart .node .verticalEdge {
        width: calc(100% - 10px);
        width: -webkit-calc(100% - 10px);
        width: -moz-calc(100% - 10px);
        left: 5px;
    }

    .orgchart .node .topEdge {
        top: -4px;
    }

    .orgchart .node .bottomEdge {
        bottom: -4px;
    }

    .orgchart .node .horizontalEdge {
        width: 15px;
        height: calc(100% - 10px);
        height: -webkit-calc(100% - 10px);
        height: -moz-calc(100% - 10px);
        top: 5px;
    }

    .orgchart .node .rightEdge {
        right: -4px;
    }

    .orgchart .node .leftEdge {
        left: -4px;
    }

    .orgchart .node .horizontalEdge::before {
        position: absolute;
        top: calc(50% - 7px);
    }

    .orgchart .node .rightEdge::before {
        right: 3px;
    }

    .orgchart .node .leftEdge::before {
        left: 3px;
    }

    .orgchart .node .toggleBtn {
        position: absolute;
        left: 5px;
        bottom: -2px;
        color: rgba(68, 157, 68, 0.6);
    }

    .orgchart .node .toggleBtn:hover {
        color: rgba(68, 157, 68, 0.8);
    }

    .oc-export-btn {
        display: inline-block;
        position: absolute;
        right: 5px;
        top: 5px;
        padding: 6px 12px;
        margin-bottom: 0;
        font-size: 14px;
        font-weight: 400;
        line-height: 1.42857143;
        text-align: center;
        white-space: nowrap;
        vertical-align: middle;
        touch-action: manipulation;
        cursor: pointer;
        user-select: none;
        color: #fff;
        background-color: #5cb85c;
        border: 1px solid transparent;
        border-color: #4cae4c;
        border-radius: 4px;
    }

    .oc-export-btn[disabled] {
        cursor: not-allowed;
        box-shadow: none;
        opacity: 0.3;
    }

    .oc-export-btn:hover,
    .oc-export-btn:focus,
    .oc-export-btn:active {
        background-color: #449d44;
        border-color: #347a34;
    }

    .orgchart~.mask {
        position: absolute;
        top: 0;
        right: 0;
        bottom: 0;
        left: 0;
        z-index: 999;
        text-align: center;
        background-color: rgba(0, 0, 0, 0.3);
    }

    .orgchart~.mask .spinner {
        position: absolute;
        top: calc(50% - 54px);
        left: calc(50% - 54px);
        color: rgba(255, 255, 255, 0.8);
        font-size: 108px;
    }

    .orgchart .node {
        transition: transform 0.3s, opacity 0.3s;
    }

    .orgchart .slide-down {
        opacity: 0;
        transform: translateY(40px);
    }

    .orgchart.l2r .node.slide-down,
    .orgchart.r2l .node.slide-down {
        transform: translateY(130px);
    }

    .orgchart .slide-up {
        opacity: 0;
        transform: translateY(-40px);
    }

    .orgchart.l2r .node.slide-up,
    .orgchart.r2l .node.slide-up {
        transform: translateY(-130px);
    }

    .orgchart .slide-right {
        opacity: 0;
        transform: translateX(130px);
    }

    .orgchart.l2r .node.slide-right,
    .orgchart.r2l .node.slide-right {
        transform: translateX(40px);
    }

    .orgchart .slide-left {
        opacity: 0;
        transform: translateX(-130px);
    }

    .orgchart.l2r .node.slide-left,
    .orgchart.r2l .node.slide-left {
        transform: translateX(-40px);
    }
</style>