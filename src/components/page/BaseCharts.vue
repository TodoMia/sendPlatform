<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item>
                    <i class="el-icon-pie-chart"></i> 用户管理
                </el-breadcrumb-item>
            </el-breadcrumb>
        </div>

        <div class="container">
            <div class="handle-box">
                <el-button type="primary" icon="el-icon-edit"  @click="handleAdd">添加用户</el-button>
            </div>
            <br>
          <el-tabs >
            <el-tab-pane label="网贷用户">
              <el-table :data="tableData" style="width: 100%"  stripe>
                  <el-table-column label="网贷用户名" prop="id"></el-table-column>
                  <el-table-column label="密码" prop="name"></el-table-column>
                  <el-table-column label="描述" prop="desc"></el-table-column>
                  <el-table-column label="操作" width="180" align="center">
                    <template slot-scope="scope">
                        <el-button  type="text" icon="el-icon-edit" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
                        <el-button type="text" icon="el-icon-delete" class="red" @click="handleDelete(scope.$index, scope.row)" >删除</el-button>
                    </template>
                </el-table-column>
              </el-table>
            </el-tab-pane>
            <el-tab-pane label="保险用户">
                <el-table :data="tableData" style="width: 100%"  stripe>
                    <el-table-column label="保险用户名" prop="id"></el-table-column>
                    <el-table-column label="密码" prop="name"></el-table-column>
                    <el-table-column label="描述" prop="desc"></el-table-column>
                    <el-table-column label="操作" width="180" align="center">
                        <template slot-scope="scope">
                            <el-button  type="text" icon="el-icon-edit" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
                            <el-button type="text" icon="el-icon-delete" class="red" @click="handleDelete(scope.$index, scope.row)" >删除</el-button>
                        </template>
                    </el-table-column>
                </el-table>
            </el-tab-pane>
            <el-tab-pane label="游戏用户">
                <el-table :data="tableData" style="width: 100%"  stripe>
                    <el-table-column label="游戏用户名" prop="id"></el-table-column>
                    <el-table-column label="密码" prop="name"></el-table-column>
                    <el-table-column label="描述" prop="desc"></el-table-column>
                    <el-table-column label="操作" width="180" align="center">
                        <template slot-scope="scope">
                            <el-button  type="text" icon="el-icon-edit" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
                            <el-button type="text" icon="el-icon-delete" class="red" @click="handleDelete(scope.$index, scope.row)" >删除</el-button>
                        </template>
                    </el-table-column>
                </el-table>
            </el-tab-pane>
            <el-tab-pane label="电商用户">
                <el-table :data="tableData" style="width: 100%"  stripe>
                    <el-table-column label="电商用户名" prop="id"></el-table-column>
                    <el-table-column label="密码" prop="name"></el-table-column>
                    <el-table-column label="描述" prop="desc"></el-table-column>
                    <el-table-column label="操作" width="180" align="center">
                        <template slot-scope="scope">
                            <el-button  type="text" icon="el-icon-edit" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
                            <el-button type="text" icon="el-icon-delete" class="red" @click="handleDelete(scope.$index, scope.row)" >删除</el-button>
                        </template>
                    </el-table-column>
                </el-table>
            </el-tab-pane>
          </el-tabs>
        </div>

        <!-- 编辑弹出框 -->
        <el-dialog :title="revise_title" :visible.sync="editVisible" v-dialogDrag  width="30%">
            <el-form ref="form" label-width="70px">
                <el-form-item label="用户分组">
                    <el-select v-model="select2" placeholder="请选择">
                        <el-option   label="网贷" value="1"></el-option>
                        <el-option   label="保险" value="2"></el-option>
                        <el-option   label="游戏" value="3"></el-option>
                        <el-option   label="电商" value="4"></el-option>
                    </el-select>
                </el-form-item>
                <el-form-item label="用户名">
                    <el-input ></el-input>
                </el-form-item>
                <el-form-item label="密码">
                    <el-input ></el-input>
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
export default {
    name: 'basecharts',
    data() {
        return {
            revise_title:'',
            select2:'',
            editVisible: false,
            tableData: [{
            id: '12987122',
            name: '好滋好味鸡蛋仔',
            category: '江浙小吃、小吃零食',
            desc: '荷兰优质淡奶，奶香浓而不腻',
            address: '上海市普陀区真北路',
            shop: '王小虎夫妻店',
            shopId: '10333'
            }, {
            id: '12987123',
            name: '好滋好味鸡蛋仔',
            category: '江浙小吃、小吃零食',
            desc: '荷兰优质淡奶，奶香浓而不腻',
            address: '上海市普陀区真北路',
            shop: '王小虎夫妻店',
            shopId: '10333'
            }, {
            id: '12987125',
            name: '好滋好味鸡蛋仔',
            category: '江浙小吃、小吃零食',
            desc: '荷兰优质淡奶，奶香浓而不腻',
            address: '上海市普陀区真北路',
            shop: '王小虎夫妻店',
            shopId: '10333'
            }, {
            id: '12987126',
            name: '好滋好味鸡蛋仔',
            category: '江浙小吃、小吃零食',
            desc: '荷兰优质淡奶，奶香浓而不腻',
            address: '上海市普陀区真北路',
            shop: '王小虎夫妻店',
            shopId: '10333'
            }]
        };
    },
    methods:{
        // 触发添加按钮
        handleAdd() {
            this.revise_title="添加用户";
            this.revise_copywriting=''
            this.revise_id=''
            this.editVisible = true;
        },
        // 编辑操作
        handleEdit(index, row) {
            this.revise_title="修改用户";
            this.revise_id=row.id;
            this.revise_copywriting=row.content;
            this.editVisible = true;
        },
        // 保存编辑
        saveEdit() {
            this.editVisible = false;
            // this.$message.success(`修改第 ${this.idx + 1} 行成功`);
            this.$message.success(`修改成功`);
            this.$set(this.tableData, this.idx, this.form);
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

    }
};
</script>

<style scoped>
.schart-box {
    display: inline-block;
    margin: 20px;
}
.schart {
    width: 600px;
    height: 400px;
}
.content-title {
    clear: both;
    font-weight: 400;
    line-height: 50px;
    margin: 10px 0;
    font-size: 22px;
    color: #1f2f3d;
}
.el-table .tablebg{
    background-color: cornflowerblue;
}
</style>