<template>
    <div id="app">
        <org-chart :datasource="ds" @node-click="selectNode">
            <!--       <template slot-scope="{ nodeData }">
        <b @click="selectNode(nodeData)">{{ nodeData.name }}</b>
      </template> -->
</org-chart>
</div>
</template>
<script>
    import '../static/css/lodash.min.js'
    import OrgChart from './components/OrganizationChartContainer.vue'

    export default {
        name: 'app',
        components: {
            OrgChart
        },
        data() {
            return {
                ds: {
                    "id": "1",
                    "nodeName": "发起人",
                    "type": "TASK_NODE",
                    "pid": "0",
                    "childNode": {
                        "id": "2",
                        "nodeName": "毛小喵1",
                        "type": "TASK_NODE",
                        "pid": "1",
                        "childNode": {
                            "id": "3",
                            "nodeName": "分支",
                            "type": "JOIN_NODE",
                            "pid": "2",
                            "conditionNodes": [{
                                    "id": "line3",
                                    "prevNodeId": "3",
                                    "nextNodeId": "4",
                                    "pid": "3",
                                    "type": "line",
                                    "childNode": {
                                        "id": "4",
                                        "nodeName": "毛小喵2",
                                        "type": "TASK_NODE",
                                        "pid": "line3",
                                        "childNode": {
                                            "id": "5",
                                            "nodeName": "分支",
                                            "type": "JOIN_NODE",
                                            "pid": "4",
                                            "conditionNodes": [{
                                                    "id": "line7",
                                                    "prevNodeId": "5",
                                                    "nextNodeId": "6",
                                                    "pid": "5",
                                                    "type": "line"
                                                },
                                                {
                                                    "id": "line8",
                                                    "prevNodeId": "5",
                                                    "nextNodeId": "6",
                                                    "pid": "5",
                                                    "type": "line"
                                                }
                                            ],
                                            "childNode": {
                                                "id": "6",
                                                "nodeName": "毛小喵3",
                                                "type": "TASK_NODE",
                                                "pid": "5",
                                                "childNode": {
                                                    "id": "7",
                                                    "nodeName": "分支",
                                                    "type": "JOIN_NODE",
                                                    "pid": "6",
                                                    "conditionNodes": [{
                                                            "id": "line10",
                                                            "prevNodeId": "7",
                                                            "nextNodeId": "8",
                                                            "pid": "7",
                                                            "type": "line"
                                                        },
                                                        {
                                                            "id": "line11",
                                                            "prevNodeId": "7",
                                                            "nextNodeId": "8",
                                                            "pid": "7",
                                                            "type": "line"
                                                        }
                                                    ]
                                                }
                                            }
                                        }
                                    }
                                },
                                {
                                    "id": "line4",
                                    "prevNodeId": "3",
                                    "nextNodeId": "8",
                                    "pid": "3",
                                    "type": "line"
                                },
                                {
                                    "id": "line5",
                                    "prevNodeId": "3",
                                    "nextNodeId": "8",
                                    "pid": "3",
                                    "type": "line"
                                }
                            ],
                            "childNode": {
                                "id": "8",
                                "nodeName": "毛小喵4",
                                "type": "TASK_NODE",
                                "pid": "3",
                                "childNode": {
                                    "id": "9",
                                    "nodeName": "分支",
                                    "type": "JOIN_NODE",
                                    "pid": "8",
                                    "conditionNodes": [{
                                            "id": "line13",
                                            "prevNodeId": "9",
                                            "nextNodeId": "10",
                                            "pid": "9",
                                            "type": "line",
                                            "childNode": {
                                                "id": "10",
                                                "nodeName": "毛小喵5",
                                                "type": "TASK_NODE",
                                                "pid": "line13"
                                            }
                                        },
                                        {
                                            "id": "line14",
                                            "prevNodeId": "9",
                                            "nextNodeId": "11",
                                            "pid": "9",
                                            "type": "line"
                                        }
                                    ],
                                    "childNode": {
                                        "id": "11",
                                        "nodeName": "结束",
                                        "type": "END_NODE",
                                        "pid": "9"
                                    }
                                }
                            }
                        }
                    }
                }
            }
        },
        methods: {
            selectNode(nodeData) {
                alert('node ' + nodeData.name + ' is selected')
            }
        }
    }
</script>
<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
    }
</style>