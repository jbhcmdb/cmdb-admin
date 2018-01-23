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
                        预生产发布-单台发布-{{project_name}}
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
    name: 'pre-single-deploy',
    components: {
    },
    data () {
        return {
            project_name:"",
            columns: [
                {
                    title: '序号',
                    type: 'index',
                    width: 80,
                    align: 'center'
                },
                {
                    title: '主机ip',
                    align: 'center',
                    key: 'ip',
                    editable: true
                },
                {
                    title: '容器',
                    align: 'center',
                    key: 'container',
                },
                {
                    title: '发布路径',
                    align: 'center',
                    key: 'path',
                    width: 180,
                    editable: true
                },
                {
                    title: '状态',
                    align: 'center',
                    key: 'status',
                    render: (h, params) => {
                        const row = params.row;
                        const color = row.status === 1 ? 'blue' : row.status === 2 ? 'green' : 'red';
                        const text = row.status === 1 ? '运行' : row.status === 2 ? '暂停' : '异常';

                        return h('Tag', {
                            props: {
                                type: 'dot',
                                color: color
                            }
                        }, text);
                    }
                },
                {
                    title: '操作',
                    align: 'center',
                    width: 180,
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
                                            this.deploy(params.index)
                                        }
                                    }
                                }, '发布'),
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
                                            this.rollback(params.index)
                                        }
                                    }
                                }, '回滚')
                            ]);
                    }
                }
            ],
            data: []
        }
    },
    methods: {
        getData () {
            this.data = tableData.table2Data;
        },

        //部署
        deploy(index){
            this.$Modal.confirm({
                    title: '确认发布！',
                    content: `
                    主机ip：${this.data[index].ip}<br>
                    容器：${this.data[index].container}<br>
                    发布路径：${this.data[index].path}<br>
                    `,
                    onOk: () => {
                        this.$Message.info('发布成功');
                    },
                    onCancel: () => {
                        this.$Message.error('发布失败');
                    }
                })
        },

        //回滚
        rollback(index){
            this.$Modal.confirm({
                    title: '确认回滚！',
                    content: `
                    主机ip：${this.data[index].ip}<br>
                    容器：${this.data[index].container}<br>
                    发布路径：${this.data[index].path}<br>
                    `,
                    onOk: () => {
                        this.$Message.info('回滚成功');
                    },
                    onCancel: () => {
                        this.$Message.error('回滚失败');
                    }
                })
        },
        
        remove (index) {
            this.data.splice(index, 1);
        }
    },
    created () {
        this.getData();
        this.project_name = this.$route.params.project_name;
    }
};
</script>
