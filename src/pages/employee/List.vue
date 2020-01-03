<template>
    <div>
        <!-- 按钮 -->
        <el-button size="small" type="success" @click="toAddHandler">添加</el-button>
        <el-button size="small" type="danger">批量删除</el-button>

        <!-- 表格 -->
        <el-table :data="employees">
            <el-table-column prop="id" label="编号"></el-table-column>
            <el-table-column prop="realname" label="姓名"></el-table-column>
            <el-table-column prop="telephone" width="120" label="手机号"></el-table-column>
            <el-table-column prop="idCard" width="200" label="身份证号"></el-table-column>
            <el-table-column prop="bankCard" width="200" label="银行卡号"></el-table-column>
            <el-table-column prop="操作" fixed="right" label="操作">
                <template v-slot="slot">
                    <a href="" @click.prevent="toDeleteHandler(slot.row.id)">删除</a>
                    <a href="" @click.prevent="toUpdateHandler(slot.row)">修改</a>
                </template>
            </el-table-column>    
        </el-table>

        <!-- 分页 -->
        <el-pagination
                background
                layout="prev, pager, next"
                :total="50">
        </el-pagination>

        <!-- 模态框 -->
        <el-dialog
            :title.sync="录入员工信息"
            :visible.sync="visible"
            width="30%">
            <span>这是一段信息</span>
            <span slot="footer" class="dialog-footer">
            <el-button size="small" @click="closeModalHandler">取 消</el-button>
            <el-button size="small" type="primary" @click="closeModalHandler">确 定</el-button>
            </span>
        </el-dialog>
    </div>
</template>

<style scoped>
    
</style>

<script>
    import request from '@/utils/request'
    //暴露接口
    export default {
        methods: {
            //重载员工数据
            loadData() {
                //this => vue实例，通过vue实例访问该实例中的数据
                let url = "http://localhost:6677/waiter/findAll";
                //箭头函数中的this指向外部函数中的this
                request.get(url).then(()=>{})
            },
            closeModalHandler() {
                this.title = "修改员工信息";
                this.visible = true;
            },
            closeModalHandler() {
                this.visible = false;
            },
            toUpdateHandler(row) {
            },
            toDeleteHandler(id) {
                //确认
                this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
                    confirmButtonText: '确定',
                    cancelButtonText: '取消',
                    type: 'warning'
                }).then(() => {
                        this.$message({
                        type: 'success',
                        message: '删除成功!'
                    });
                })
            }
        },

        data() {
            return {
                visible:false,
                employees:[]
            }
        },
        created() {
            //在页面加载出的时候加载数据
            this.loadData();
        }

    }
</script>