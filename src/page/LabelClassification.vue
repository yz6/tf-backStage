<template>
    <div class="labelBox">
        <el-tabs v-model="activeName" @tab-click="handleClick">
            <el-tab-pane label="视频标签" name="first">
                    <span>共8条</span>
                <el-button @click="addLebel" class="addWz" style="margin: 0 15px;float: right" type="primary" >&nbsp; 新建标签 &nbsp;</el-button>
                <el-input  class="anchorName" style="width: 150px;float: right" v-model="anchorName" placeholder="标签名称"><el-button slot="append" icon="el-icon-search"></el-button></el-input>
                <div style="margin-top: 20px">
                    <el-table
                        :data="tableData"
                        border
                        style="width: 100%;">
                        <el-table-column
                            prop="name"
                            label="标签名称">
                        </el-table-column>
                        <el-table-column
                            prop="num"
                            label="相关视频"
                            width="180">
                        </el-table-column>
                        <el-table-column
                            label="操作"
                            width="180">
                            <template slot-scope="scope">
                                <i style="font-size: 18px;margin: 0 5px;cursor: pointer" class="el-icon-edit-outline"></i>
                                <i style="font-size: 18px;margin: 0 5px;cursor: pointer"  class="el-icon-delete"></i>
                            </template>
                        </el-table-column>
                    </el-table>
                </div>
            </el-tab-pane>
            <el-tab-pane label="文章分类" name="second">
                <span>共8条</span>
                <el-button @click="addClassification" class="addWz" style="margin: 0 15px;float: right" type="primary" >&nbsp; 新建分类 &nbsp;</el-button>
                <el-input  class="anchorName" style="width: 150px;float: right" v-model="anchorName" placeholder="类别名称"><el-button slot="append" icon="el-icon-search"></el-button></el-input>
                <div style="margin-top: 20px">
                    <el-table
                        :data="tableData"
                        border
                        style="width: 100%;">
                        <el-table-column
                            prop="name"
                            label="文章类别">
                        </el-table-column>
                        <el-table-column
                            prop="num"
                            label="相关文章"
                            width="180">
                        </el-table-column>
                        <el-table-column
                            label="操作"
                            width="180">
                            <template slot-scope="scope">
                                <i style="font-size: 18px;margin: 0 5px;cursor: pointer" class="el-icon-edit-outline"></i>
                                <i style="font-size: 18px;margin: 0 5px;cursor: pointer"  class="el-icon-delete"></i>
                            </template>
                        </el-table-column>
                    </el-table>
                </div>
            </el-tab-pane>

        </el-tabs>
        <div class="block paginationBox">
            <el-pagination
                @current-change="handleCurrentChange"
                :current-page="lebelCurrentPage"
                :page-size="100"
                layout="total, prev, pager, next, jumper"
                :total="tableData.length">
            </el-pagination>
        </div>
    </div>
</template>

<script>
    export default {
        name: "label-classification",
        data() {
            return {
                activeName: 'first',
                lebelCurrentPage:1,
                tableData: [{
                    name: '娱乐搞笑',
                    num: 30,
                }, {
                    name: '美女',
                    num: 21,
                },{
                    name: '教学',
                    num: 107,
                },{
                    name: '王者荣耀',
                    num: 10,
                },{
                    name: '吃鸡',
                    num: 200,
                },{
                    name: '五杀',
                    num: 6,
                },{
                    name: '英雄联盟',
                    num: 83,
                },{
                    name: '职业选手',
                    num: 77,
                }]
            };
        },
        methods:{
            handleClick(tab, event) {
                console.log(tab, event);
            },
            //分页事件
            handleCurrentChange(val) {
                console.log(`当前页: ${val}`);
            },
            //
            addLebel(){
                this.$prompt('请输入新标签名称', '提示', {
                    confirmButtonText: '确定',
                    cancelButtonText: '取消',
                }).then(({ value }) => {
                    this.$message({
                        type: 'success',
                        message:   value
                    });
                })
            },
            addClassification(){
                this.$prompt('请输入新文章类别', '提示', {
                    confirmButtonText: '确定',
                    cancelButtonText: '取消',
                }).then(({ value }) => {
                    this.$message({
                        type: 'success',
                        message:   value
                    });
                })
            },
        },
    }
</script>

<style lang="less">
    .labelBox{
    padding: 10px 20px;
        .el-tabs__item{
            font-size: 16px;
        }
    }
    .labelBox .el-input__inner{
        height: 30px;
        line-height: 30px;
    }
    .labelBox .el-button{
       padding: 7px 10px;
        border-radius: 0;
    }
    .labelBox .el-input-group__append{
        padding: 0 15px;
    }
    .labelBox {
        td{
            text-align: center;
        }
        th{
            text-align: center;
        }
        th:first-child{
            text-align: left;
        }
        tr{
            td:first-child{
                text-align: left;
            }
        }
    }
</style>
