<template>
    <div class="tabs">
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item>
                    <i class="el-icon-lx-cascades"></i> 失败重提
                </el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <div class="handle-box">
                <!-- <el-button type="primary"  icon="el-icon-delete" class="handle-del mr10" @click="delAllSelection">批量删除</el-button> -->
                <!-- <el-input v-model="query.name" placeholder="用户名" class="handle-input mr10"></el-input> -->
                <!-- <el-button type="primary" icon="el-icon-edit" @click="handleAdd">添加预提模型</el-button> -->
            </div>
            <el-table
                :data="tableData"
                border
                class="table"
                ref="multipleTable"
                header-cell-class-name="table-header"
                @selection-change="handleSelectionChange"
            >
                <el-table-column prop="date" label="模型名称" ></el-table-column>
                <el-table-column prop="money" label="模型类型" ></el-table-column>
                <el-table-column prop="name" label="链接名称"  ></el-table-column>
                <el-table-column prop="money" label="投放网关"  ></el-table-column>
                <!-- <el-table-column prop="address" label="投放数据"  ></el-table-column> -->
                <!-- <el-table-column prop="timer" label="发送时间" width="160"  ></el-table-column> -->
                <el-table-column label="状态" align="center">
                    <template slot-scope="scope">
                        <el-tag
                            :type="scope.row.state==='成功'?'success':(scope.row.state==='失败'?'danger':'')"
                        >{{scope.row.state}}</el-tag>
                    </template>
                </el-table-column>
                <el-table-column prop="" label="模型描述"  ></el-table-column>
                <el-table-column label="操作"  align="center">
                    <template slot-scope="scope">
                        <el-button
                            type="text"
                            icon="el-icon-refresh-right"
                            @click="handleEdit(scope.$index, scope.row)"
                        >重提</el-button>
                    </template>
                </el-table-column>
            </el-table>
            <div class="pagination">
                <el-pagination
                    background
                    layout="total, prev, pager, next"
                    :current-page="query.pageIndex"
                    :page-size="query.pageSize"
                    :total="pageTotal"
                    @current-change="handlePageChange"
                ></el-pagination>
            </div>
        </div>

       
    </div>
</template>

<script>
import { fetchData } from '../../api/index';
export default {
    name: 'basetable',
    data() {
        return {
            timer:'',
            select2:'',
            select:'',
            input3:'',
            revise_title:'',
            revise_id:'',
            revise_copywriting:'',
            query: {
                address: '',
                name: '',
                pageIndex: 1,
                pageSize: 10
            },
            tableData: [],
            multipleSelection: [],
            delList: [],
            editVisible: false,
            pageTotal: 0,
            form: {},
            id: -1
        };
    },
    created() {
        this.getData();
    },
    methods: {
        // 获取 easy-mock 的模拟数据
        getData() {
            fetchData(this.query).then(res => {
                console.log(res);
                this.tableData = res.list;
                this.pageTotal = res.pageTotal || 50;
            });
        },
        // 编辑操作
        handleEdit(index, row) {

        },
        // 分页导航
        handlePageChange(val) {
            this.$set(this.query, 'pageIndex', val);
            this.getData();
        }
    }
};
</script>

<style scoped>
.handle-box {
    margin-bottom: 20px;
}

.handle-select {
    width: 120px;
}

.handle-input {
    width: 300px;
    display: inline-block;
}
.table {
    width: 100%;
    font-size: 14px;
}
.red {
    color: #ff0000;
}
.mr10 {
    margin-right: 10px;
}
.table-td-thumb {
    display: block;
    margin: auto;
    width: 40px;
    height: 40px;
}

.tabs >>> .el-input-group__prepend {
    width: 200px;
}
.el-select {
    width: 100%;
}


.tabs >>> .el-select-dropdown {
    width: 42%;
}
.tabs >>> .el-select-dropdown__item {
    font-size: 14px;
    padding: 0 20px;
    position: relative;
    /* white-space: nowrap; */
    white-space: normal;
    overflow: auto;
    /* text-overflow: ellipsis; */
    color: #606266;
    /* height: 34px; */
    line-height: 34px;
    /* -webkit-box-sizing: border-box; */
    box-sizing: border-box;
    cursor: pointer;
}
.el-date-editor.el-input, .el-date-editor.el-input__inner{
    width: 100%;
}
.tabs >>> .el-upload ,.tabs >>> .el-upload-dragger{
    width: 100%;
}
</style>
