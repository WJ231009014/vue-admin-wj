<template>
    <div>
        <!-- 按钮 -->
        <el-button type="success" size="small" @click="toAddHandler">添加</el-button> 
        <el-button type="danger" size="small">批量删除</el-button>

        <!-- 表格 -->
        <el-table :data="categorys">
        <el-table-column prop="id" label="序号"></el-table-column>
        <el-table-column prop="name" label="栏目名称"></el-table-column>
        <el-table-column prop="parentId" label="父栏目"></el-table-column>
        <el-table-column label="操作">
            <template v-slot="slot">
            <a href="" @click.prevent="toDeleteHandler(slot.row.id)">删除</a>
            <a href="" @click.prevent="toUpdateHandler">修改</a>
            </template>
        </el-table-column>
        </el-table>

        <!-- 模态框 -->
        <el-dialog
        title="录入栏目信息"
        :visible.sync="visible"
        width="60%">
            ---{{form}}
        <el-form :model="form" label-width="80px">
            <el-form-item label="序号">
            <el-input v-model="form.username"></el-input>
            </el-form-item>
            <el-form-item label="栏目名称">
            <el-input v-model="form.password"></el-input>
            </el-form-item>
            <el-form-item label="父栏目">
            <el-input v-model="form.realname"></el-input>
            </el-form-item>
        </el-form>

        <span slot="footer" class="dialog-footer">
            <el-button size="small" @click="closeModalHandler">取 消</el-button>
            <el-button size="small" type="primary" @click="submitHandler">确 定</el-button>
        </span>
        </el-dialog>
    </div>
</template>

<script>
export default {
    // 用于存放网页中需要调用的方法
    methods:{
        loadData(){
        let url = "http://localhost:6677/category/findAll"
        request.get(url).then((response)=>{
            // 将查询结果设置到customers中，this指向外部函数的this
            this.customers = response.data;
        })
        },
        submitHandler(){
        //this.form 对象 ---字符串--> 后台 {type:'customer',age:12}
        // json字符串 '{"type":"customer","age":12}'
        // request.post(url,this.form)
        // 查询字符串 type=customer&age=12
        // 通过request与后台进行交互，并且要携带参数
        let url = "http://localhost:6677/customer/saveOrUpdate";
        request({
            url,
            method:"POST",
            headers:{
            "Content-Type":"application/x-www-form-urlencoded"
            },
            data:querystring.stringify(this.form)
        }).then((response)=>{
            // 模态框关闭
            this.closeModalHandler();
            // 刷新
            this.loadData();
            // 提示消息
            this.$message({
            type:"success",
            message:response.message
            })
        })

        },
        toDeleteHandler(id){
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
        
        },
        toUpdateHandler(){
        this.visible = true;
        },
        closeModalHandler(){
        this.visible = false;
        },
        toAddHandler(){
        this.visible = true;
        }
    },
    // 用于存放要向网页中显示的数据
    data(){
        return {
            visible:false,
            customers:[],
            form:{
                type:"customer"
            }
        }
    },
    created(){
        // this为当前vue实例对象
        // vue实例创建完毕 
        this.loadData()

    }
}
</script>

<style scoped>

</style>