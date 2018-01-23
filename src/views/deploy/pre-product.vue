<style lang="less">
    @import '../../styles/common.less';
</style>

<template>
    <div>
        <Row>
            <Col class="padding-left-10">
                <Card>
                    <p slot="title">
                        <Icon type="ios-book"></Icon>
                        预生产发布
                    </p>
                    <div>
                        <Table :columns="columns" :data="data"></Table>
                    </div>
                </Card>
            </Col>
        </Row>
    </div>
</template>

<script>
import tableData from './table_data.js';
export default {
    name: 'pre-product',
    components: {
    },
    data () {
        return {
            columns: [
                {
                    title: '序号',
                    type: 'index',
                    width: 80,
                    align: 'center'
                },
                {
                    title: '应用名称',
                    align: 'center',
                    key: 'project',
                    editable: true
                },
                {
                    title: '项目组',
                    align: 'center',
                    key: 'project',
                },
                {
                    title: '开发负责人',
                    align: 'center',
                    key: 'name',
                    editable: true
                },
                {
                    title: '状态',
                    align: 'center',
                    key: 'status',
                    editable: true
                },
                {
                    title: '操作',
                    align: 'center',
                    width: 350,
                    key: 'handle',
                    render: (h, params) => {
                            return h('div', [
                                h('Button', {
                                    props: {
                                        type: 'primary',
                                        size: 'small'
                                    },
                                    style: {
                                        marginRight: '5px'
                                    },
                                    on: {
                                        click: () => {
                                            this.allDeploy(params.index)
                                        }
                                    }
                                }, '批量发布'),
                                h('Button', {
                                    props: {
                                        type: 'error',
                                        size: 'small'
                                    },
                                    style: {
                                        marginRight: '5px'
                                    },
                                    on: {
                                        click: () => {
                                            this.allRollback(params.index)
                                        }
                                    }
                                }, '批量回滚'),
                                h('Button', {
                                    props: {
                                        type: 'primary',
                                        size: 'small'
                                    },
                                    style: {
                                        marginRight: '5px'
                                    },
                                    on: {
                                        click: () => {
                                            this.singleDeploy(params.index,params)
                                        }
                                    }
                                }, '单台发布回滚')
                            ]);
                    }
                }
            ],
            data: []
        }
    },
    methods: {
        getData () {
            this.data = tableData.table1Data;
        },

        //批量部署
        allDeploy(index){
            this.$Modal.confirm({
                    title: '确认批量发布！',
                    content: `
                    应用名称：${this.data[index].project}<br>
                    项目组：${this.data[index].group}<br>
                    开发负责人：${this.data[index].name}<br>
                    批量发布主机：10.70.21.32，10.70.21.33`,
                    onOk: () => {
                        this.$Message.info('批量发布成功');
                    },
                    onCancel: () => {
                        this.$Message.error('批量发布失败');
                    }
                })
        },

        //批量回滚
        allRollback(index){
            this.$Modal.confirm({
                    title: '确认批量回滚！',
                    content: `
                    应用名称：${this.data[index].project}<br>
                    项目组：${this.data[index].group}<br>
                    开发负责人：${this.data[index].name}<br>
                    批量发布主机：10.70.21.32，10.70.21.33`,
                    onOk: () => {
                        this.$Message.info('批量回滚成功');
                    },
                    onCancel: () => {
                        this.$Message.error('批量回滚失败');
                    }
                })
        },
        singleDeploy(index,params) {
            let argu = { project_name: params.row.project };
            this.$router.push({
                name: 'single-deploy',
                params: argu
            });
        },
        remove (index) {
            this.data.splice(index, 1);
        }
    },
    created () {
        this.getData();
    }
};
</script>
