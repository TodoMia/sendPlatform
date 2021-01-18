<template>
    <div class="tabs">
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item>
                    <i class="el-icon-lx-cascades"></i> 预提
                </el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <div class="handle-box">
                <!-- <el-button type="primary"  icon="el-icon-delete" class="handle-del mr10" @click="delAllSelection">批量删除</el-button> -->
                <!-- <el-input v-model="query.name" placeholder="用户名" class="handle-input mr10"></el-input> -->
                <el-button type="primary" icon="el-icon-edit" @click="handleAdd">添加预提模型</el-button>
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
                <el-table-column prop="address" label="投放数据"  ></el-table-column>
                <el-table-column prop="timer" label="发送时间" width="160"  ></el-table-column>
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
                            icon="el-icon-edit"
                            @click="handleEdit(scope.$index, scope.row)"
                        >编辑</el-button>
                        <el-button
                            type="text"
                            icon="el-icon-delete"
                            class="red"
                            @click="handleDelete(scope.$index, scope.row)"
                        >删除</el-button>
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

        <!-- 编辑弹出框 -->
        <el-dialog :title="revise_title" :visible.sync="editVisible" v-dialogDrag  width="40%">
            <el-form ref="form" label-width="70px">
                <!-- <el-input  v-model="revise_copywriting" maxlength="70" type="textarea" rows="5" show-word-limit></el-input> -->
                <el-form-item label="模型名称">
                    <el-input v-model="form.name"></el-input>
                </el-form-item>
                <el-form-item label="发送时间">
                    <el-date-picker
                    v-model="timer"
                    type="datetime"
                    placeholder="选择日期时间"
                    default-time="8:00:00">
                    </el-date-picker>
                </el-form-item>
                <el-form-item label="模型类型">
                    <el-select v-model="select2" placeholder="请选择">
                        <el-option v-for="item in tableData" :key="item.id" :label="item.money" :value="item.id"></el-option>
                      </el-select>
                </el-form-item>
                <el-form-item label="链接名称">
                    <el-select v-model="select2" placeholder="请选择">
                        <el-option v-for="item in tableData" :key="item.id" :label="item.name" :value="item.id"></el-option>
                      </el-select>
                </el-form-item>
                <el-form-item label="链接名称">
                    <el-select v-model="select2" placeholder="请选择">
                        <el-option v-for="item in tableData" :key="item.id" :label="item.money" :value="item.id"></el-option>
                      </el-select>
                </el-form-item>
                <el-form-item label="投放数据">
                    <el-upload
                        class="upload-demo"
                        drag
                        action="http://119.23.48.180:8088/api/v1/"
                        :on-error="uploagerror"
                        :on-success="handleAvatarSuccess"
                        >
                        <i class="el-icon-upload"></i>
                        <div class="el-upload__text">将文件拖到此处，或<em>点击上传</em></div>
                        <!-- <div class="el-upload__tip" slot="tip">只能上传jpg/png文件，且不超过500kb</div> -->
                    </el-upload>
                </el-form-item>
                <el-form-item label="模型描述">
                    <el-input v-model="form.name"></el-input>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button @click="editVisible = false">取 消</el-button>
                <el-button type="primary" @click="saveEdit">确 定</el-button>
            </span>
        </el-dialog>
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
        uploagerror(){

        },
        handleAvatarSuccess(){

        },
        // 获取 easy-mock 的模拟数据
        getData() {
            fetchData(this.query).then(res => {
                console.log(res);
                this.tableData = res.list;
                this.pageTotal = res.pageTotal || 50;
            });
        },
        // 触发搜索按钮
        handleAdd() {
            this.revise_title="添加模型"
            this.editVisible = true;
        },
        // 删除操作
        handleDelete(index, row) {
            // 二次确认删除
            this.$confirm('确定要删除吗？', '提示', {
                type: 'warning'
            })
                .then(() => {
                    this.$message.success('删除成功');
                    this.tableData.splice(index, 1);
                })
                .catch(() => {});
        },
        // 多选操作
        handleSelectionChange(val) {
            this.multipleSelection = val;
        },
        delAllSelection() {
            const length = this.multipleSelection.length;
            let str = '';
            this.delList = this.delList.concat(this.multipleSelection);
            for (let i = 0; i < length; i++) {
                str += this.multipleSelection[i].name + ' ';
            }
            this.$message.error(`删除了${str}`);
            this.multipleSelection = [];
        },
        // 编辑操作
        handleEdit(index, row) {
            this.revise_title="修改模型"
            this.revise_id=row.id;
            this.revise_copywriting=row.content;
            this.editVisible = true;
        },
        // 保存编辑
        saveEdit() {
            this.editVisible = false;
            // this.$message.success(`修改第 ${this.idx + 1} 行成功`);
            this.$message.success(`修改 成功`);
            this.$set(this.tableData, this.idx, this.form);
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
