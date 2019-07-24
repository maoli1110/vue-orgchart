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
                "name": "发起人",
                "type": "start",
                "nodeId": "sid-startevent",
                "childNode": {
                    "name": "UNKNOWN",
                    "type": "approver",
                    "prevId": "sid-startevent",
                    "nodeId": "c144_655d",
                    "properties": {
                        "activateType": "ONE_BY_ONE",
                        "agreeAll": false,
                        "actionerRules": [{
                            "type": "target_approval",
                            "approvals": [{
                                "userName": "毛小喵",
                                "workNo": "manager3770"
                            }],
                            "isEmpty": false
                        }]
                    },
                    "childNode": {
                        "type": "route",
                        "name": "route",
                        "prevId": "c144_655d",
                        "nodeId": "601a_733d",
                        "conditionNodes": [{
                                "name": "条件1",
                                "type": "condition",
                                "prevId": "601a_733d",
                                "nodeId": "b9e1_faea",
                                "properties": {
                                    "conditions": [
                                        [{
                                                "type": "dingtalk_actioner_range_condition",
                                                "paramKey": "MoneyField-J8R09VIT",
                                                "paramLabel": "住宿费（元）",
                                                "lowerBound": "",
                                                "upperBound": 100,
                                                "isEmpty": false,
                                                "valid": "valid",
                                                "lowerBoundNotEqual": "",
                                                "upperBoundEqual": "",
                                                "boundEqual": "",
                                                "key": "l"
                                            },
                                            {
                                                "type": "dingtalk_actioner_range_condition",
                                                "paramKey": "MoneyField-J8R09VIU",
                                                "paramLabel": "餐饮费（元）",
                                                "lowerBound": "",
                                                "upperBound": "",
                                                "isEmpty": false,
                                                "valid": "valid",
                                                "lowerBoundNotEqual": "",
                                                "upperBoundEqual": 200,
                                                "boundEqual": "",
                                                "key": "le"
                                            }
                                        ]
                                    ]
                                },
                                "childNode": {
                                    "name": "审批人",
                                    "type": "approver",
                                    "prevId": "b9e1_faea",
                                    "nodeId": "sid-1234_5678",
                                    "properties": {
                                        "activateType": "ONE_BY_ONE",
                                        "agreeAll": false,
                                        "allowTaskAppend": false,
                                        "allowTaskRedirect": true,
                                        "actionerRules": [{
                                            "type": "target_originator",
                                            "isEmpty": false
                                        }]
                                    },
                                    "childNode": {
                                        "type": "route",
                                        "name": "route",
                                        "prevId": "sid-1234_5678",
                                        "nodeId": "8fb0_cb12",
                                        "conditionNodes": [{
                                                "name": "条件1-1",
                                                "type": "condition",
                                                "prevId": "8fb0_cb12",
                                                "nodeId": "4e19_631e",
                                                "properties": {
                                                    "conditions": [
                                                        [{
                                                                "type": "dingtalk_actioner_range_condition",
                                                                "paramKey": "MoneyField-J8R09VIT",
                                                                "paramLabel": "住宿费（元）",
                                                                "lowerBound": 2,
                                                                "upperBound": "",
                                                                "isEmpty": false,
                                                                "valid": "valid"
                                                            },
                                                            {
                                                                "type": "dingtalk_actioner_range_condition",
                                                                "paramKey": "MoneyField-J8R09VIU",
                                                                "paramLabel": "餐饮费（元）",
                                                                "lowerBound": 2,
                                                                "upperBound": "",
                                                                "isEmpty": false,
                                                                "valid": "valid"
                                                            }
                                                        ]
                                                    ]
                                                },
                                                "childNode": {
                                                    "type": "route",
                                                    "name": "route",
                                                    "prevId": "4e19_631e",
                                                    "nodeId": "1218_7151",
                                                    "conditionNodes": [{
                                                            "name": "条件1-1-1",
                                                            "type": "condition",
                                                            "prevId": "1218_7151",
                                                            "nodeId": "d4bc_b69f",
                                                            "properties": {
                                                                "conditions": [
                                                                    [{
                                                                        "type": "dingtalk_actioner_range_condition",
                                                                        "paramKey": "MoneyField-J8R09VIT",
                                                                        "paramLabel": "住宿费（元）",
                                                                        "lowerBound": 1,
                                                                        "upperBound": "",
                                                                        "isEmpty": false,
                                                                        "valid": "valid"
                                                                    }]
                                                                ]
                                                            }
                                                        },
                                                        {
                                                            "name": "条件1-1-2",
                                                            "type": "condition",
                                                            "prevId": "1218_7151",
                                                            "nodeId": "dac7_4149",
                                                            "properties": {
                                                                "conditions": [
                                                                    [{
                                                                        "type": "dingtalk_actioner_range_condition",
                                                                        "paramKey": "MoneyField-J8R09VIT",
                                                                        "paramLabel": "住宿费（元）",
                                                                        "lowerBound": 2,
                                                                        "upperBound": "",
                                                                        "isEmpty": false,
                                                                        "valid": "valid"
                                                                    }]
                                                                ]
                                                            }
                                                        }
                                                    ],
                                                    "properties": {

                                                    }
                                                }
                                            },
                                            {
                                                "name": "条件1-2",
                                                "type": "condition",
                                                "prevId": "8fb0_cb12",
                                                "nodeId": "b046_b820",
                                                "properties": {
                                                    "conditions": [
                                                        [{
                                                                "type": "dingtalk_actioner_range_condition",
                                                                "paramKey": "MoneyField-J8R09VIT",
                                                                "paramLabel": "住宿费（元）",
                                                                "lowerBound": 1,
                                                                "upperBound": "",
                                                                "isEmpty": false,
                                                                "valid": "valid"
                                                            },
                                                            {
                                                                "type": "dingtalk_actioner_range_condition",
                                                                "paramKey": "MoneyField-J8R09VIU",
                                                                "paramLabel": "餐饮费（元）",
                                                                "lowerBound": 1,
                                                                "upperBound": "",
                                                                "isEmpty": false,
                                                                "valid": "valid"
                                                            }
                                                        ]
                                                    ]
                                                }
                                            }
                                        ],
                                        "properties": {

                                        },
                                        "childNode": {
                                            "name": "UNKNOWN",
                                            "type": "approver",
                                            "prevId": "8fb0_cb12",
                                            "nodeId": "c564_69d1",
                                            "properties": {
                                                "activateType": "ONE_BY_ONE",
                                                "agreeAll": false,
                                                "actionerRules": [{
                                                    "type": "target_approval",
                                                    "approvals": [{
                                                        "userName": "毛小喵",
                                                        "workNo": "manager3770"
                                                    }],
                                                    "isEmpty": false
                                                }]
                                            },
                                            "childNode": {
                                                "type": "route",
                                                "name": "route",
                                                "prevId": "c564_69d1",
                                                "nodeId": "940b_8ed1",
                                                "conditionNodes": [{
                                                        "name": "条件1-1-1-1",
                                                        "type": "condition",
                                                        "prevId": "940b_8ed1",
                                                        "nodeId": "9a7a_d0be",
                                                        "properties": {
                                                            "conditions": [
                                                                [{
                                                                        "type": "dingtalk_actioner_range_condition",
                                                                        "paramKey": "MoneyField-J8R09VIT",
                                                                        "paramLabel": "住宿费（元）",
                                                                        "lowerBound": 2,
                                                                        "upperBound": "",
                                                                        "isEmpty": false,
                                                                        "valid": "valid"
                                                                    },
                                                                    {
                                                                        "type": "dingtalk_actioner_range_condition",
                                                                        "paramKey": "MoneyField-J8R09VIU",
                                                                        "paramLabel": "餐饮费（元）",
                                                                        "lowerBound": 3,
                                                                        "upperBound": "",
                                                                        "isEmpty": false,
                                                                        "valid": "valid"
                                                                    }
                                                                ]
                                                            ]
                                                        }
                                                    },
                                                    {
                                                        "name": "条件1-1-1-2",
                                                        "type": "condition",
                                                        "prevId": "940b_8ed1",
                                                        "nodeId": "b103_54fa",
                                                        "properties": {
                                                            "conditions": [
                                                                [{
                                                                        "type": "dingtalk_actioner_range_condition",
                                                                        "paramKey": "MoneyField-J8R09VIT",
                                                                        "paramLabel": "住宿费（元）",
                                                                        "lowerBound": 4,
                                                                        "upperBound": "",
                                                                        "isEmpty": false,
                                                                        "valid": "valid"
                                                                    },
                                                                    {
                                                                        "type": "dingtalk_actioner_range_condition",
                                                                        "paramKey": "MoneyField-J8R09VIU",
                                                                        "paramLabel": "餐饮费（元）",
                                                                        "lowerBound": 5,
                                                                        "upperBound": "",
                                                                        "isEmpty": false,
                                                                        "valid": "valid"
                                                                    }
                                                                ]
                                                            ]
                                                        }
                                                    }
                                                ],
                                                "properties": {

                                                }
                                            }
                                        }
                                    }
                                }
                            },
                            {
                                "name": "条件2",
                                "type": "condition",
                                "prevId": "601a_733d",
                                "nodeId": "c82c_bb27",
                                "properties": {
                                    "conditions": [
                                        [{
                                                "type": "dingtalk_actioner_range_condition",
                                                "paramKey": "MoneyField-J8R09VIT",
                                                "paramLabel": "住宿费（元）",
                                                "lowerBound": 200,
                                                "upperBound": "",
                                                "isEmpty": false,
                                                "valid": "valid"
                                            },
                                            {
                                                "type": "dingtalk_actioner_range_condition",
                                                "paramKey": "MoneyField-J8R09VIU",
                                                "paramLabel": "餐饮费（元）",
                                                "lowerBound": 300,
                                                "upperBound": "",
                                                "isEmpty": false,
                                                "valid": "valid"
                                            }
                                        ]
                                    ]
                                }
                            },
                            {
                                "name": "条件3",
                                "type": "condition",
                                "prevId": "601a_733d",
                                "nodeId": "9080_a142",
                                "properties": {
                                    "conditions": [
                                        [{
                                                "type": "dingtalk_actioner_range_condition",
                                                "paramKey": "MoneyField-J8R09VIT",
                                                "paramLabel": "住宿费（元）",
                                                "lowerBound": 400,
                                                "upperBound": "",
                                                "isEmpty": false,
                                                "valid": "valid"
                                            },
                                            {
                                                "type": "dingtalk_actioner_range_condition",
                                                "paramKey": "MoneyField-J8R09VIU",
                                                "paramLabel": "餐饮费（元）",
                                                "lowerBound": 500,
                                                "upperBound": "",
                                                "isEmpty": false,
                                                "valid": "valid"
                                            }
                                        ]
                                    ]
                                }
                            }
                        ],
                        "properties": {

                        },
                        "childNode": {
                            "name": "UNKNOWN",
                            "type": "approver",
                            "prevId": "601a_733d",
                            "nodeId": "3e3c_aa94",
                            "properties": {
                                "activateType": "ONE_BY_ONE",
                                "agreeAll": false,
                                "actionerRules": [{
                                    "type": "target_approval",
                                    "approvals": [{
                                        "userName": "毛小喵",
                                        "workNo": "manager3770"
                                    }],
                                    "isEmpty": false
                                }]
                            },
                            "childNode": {
                                "type": "route",
                                "name": "route",
                                "prevId": "3e3c_aa94",
                                "nodeId": "c144_3b77",
                                "conditionNodes": [{
                                        "name": "条件1-end",
                                        "type": "condition",
                                        "prevId": "c144_3b77",
                                        "nodeId": "f115_cdf3",
                                        "properties": {
                                            "conditions": [
                                                [{
                                                        "type": "dingtalk_actioner_range_condition",
                                                        "paramKey": "MoneyField-J8R09VIT",
                                                        "paramLabel": "住宿费（元）",
                                                        "lowerBound": 1,
                                                        "upperBound": "",
                                                        "isEmpty": false,
                                                        "valid": "valid"
                                                    },
                                                    {
                                                        "type": "dingtalk_actioner_range_condition",
                                                        "paramKey": "MoneyField-J8R09VIU",
                                                        "paramLabel": "餐饮费（元）",
                                                        "lowerBound": 1,
                                                        "upperBound": "",
                                                        "isEmpty": false,
                                                        "valid": "valid"
                                                    }
                                                ]
                                            ]
                                        },
                                        "childNode": {
                                            "name": "UNKNOWN",
                                            "type": "approver",
                                            "prevId": "f115_cdf3",
                                            "nodeId": "e822_bc7e",
                                            "properties": {
                                                "activateType": "ONE_BY_ONE",
                                                "agreeAll": false,
                                                "actionerRules": [{
                                                    "type": "target_approval",
                                                    "approvals": [{
                                                        "userName": "毛小喵",
                                                        "workNo": "manager3770"
                                                    }],
                                                    "isEmpty": false
                                                }]
                                            }
                                        }
                                    },
                                    {
                                        "name": "条件2-end",
                                        "type": "condition",
                                        "prevId": "c144_3b77",
                                        "nodeId": "8a4d_a338",
                                        "properties": {
                                            "conditions": [
                                                [{
                                                        "type": "dingtalk_actioner_range_condition",
                                                        "paramKey": "MoneyField-J8R09VIT",
                                                        "paramLabel": "住宿费（元）",
                                                        "lowerBound": 1,
                                                        "upperBound": "",
                                                        "isEmpty": false,
                                                        "valid": "valid"
                                                    },
                                                    {
                                                        "type": "dingtalk_actioner_range_condition",
                                                        "paramKey": "MoneyField-J8R09VIU",
                                                        "paramLabel": "餐饮费（元）",
                                                        "lowerBound": 1,
                                                        "upperBound": "",
                                                        "isEmpty": false,
                                                        "valid": "valid"
                                                    }
                                                ]
                                            ]
                                        }
                                    }
                                ],
                                "properties": {

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