<template>
    <div class="tabs">
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item>
                    <i class="el-icon-lx-cascades"></i> 产品链接
                </el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <div class="handle-box">
                <!-- <el-button type="primary"  icon="el-icon-delete" class="handle-del mr10" @click="delAllSelection">批量删除</el-button> -->
                <!-- <el-input v-model="query.name" placeholder="用户名" class="handle-input mr10"></el-input> -->
                <el-button type="primary" icon="el-icon-edit" @click="handleAdd">添加链接</el-button>
            </div>
            <el-table
                :data="tableData"
                border
                class="table"
                ref="multipleTable"
                header-cell-class-name="table-header"
                @selection-change="handleSelectionChange"
            >
                <el-table-column prop="name" label="产品名称" width="155"></el-table-column>
                <el-table-column prop="thumb" label="投放链接" width="255" ></el-table-column>
                <el-table-column prop="content" label="短信文本"></el-table-column>
                <el-table-column label="操作" width="180" align="center">
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
                <el-form-item label="产品名称">
                    <el-input v-model="form.name"></el-input>
                </el-form-item>
                <el-form-item label="投放链接">
                    <el-input placeholder="请输入内容" v-model="input3" class="input-with-select">
                        <el-select v-model="select" slot="prepend"  placeholder="请选择">
                          <el-option label="http://www.youqianhua.com" value="1"></el-option>
                          <el-option label="http://www.paipaidai.com" value="2"></el-option>
                          <el-option label="http://www.huanbei.com" value="3"></el-option>
                        </el-select>
                      </el-input>
                </el-form-item>
                <el-form-item label="文案模板">
                    <el-select v-model="select2" placeholder="请选择">
                        <el-option v-for="item in tableData" :key="item.id" :label="item.content" :value="item.id"></el-option>
                      </el-select>
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
        // 触发搜索按钮
        handleAdd() {
            this.revise_title="添加链接"
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
            this.revise_title="修改链接"
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
</style>
