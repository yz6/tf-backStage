<template>
   <div class="wzGl">
       <div v-if="listPage">
           <div class="listPage">
                   <p style="float: left" class="content-title">文章(共4条)</p>
                   <div style="float: right;padding: 20px 10px 0 0;display: flex;margin-bottom: 20px;width: 350px">
                       <el-input
                           placeholder="主播名"
                           v-model="zbName">
                       </el-input>
                       <el-button  style="width: 60px" class="el-icon-search"></el-button>
                       <el-button @click="goAddPage" class="addWz" style="margin: 0 15px;width: 180px;" type="primary" >&nbsp; 新建文章 &nbsp;</el-button>
                   </div>
               <div style="padding: 0 20px">
                   <el-table
                       :data="tableData"
                       border
                       style="width: 100%;">
                       <el-table-column
                           label="内容">
                           <template  slot-scope="scope">
                               <div class="tableContentBox">
                                   <img  class="tableImg" :src="scope.row.content.imgUrl" alt="">
                                   <a href="#"  style="margin-left: 40px">{{scope.row.content.title}}</a>
                               </div>

                           </template>
                       </el-table-column>
                       <el-table-column
                           prop="name"
                           label="关联主播"
                           width="100">
                       </el-table-column>
                       <el-table-column
                           label="更新时间"
                       width="150">
                           <template slot-scope="scope">
                               <span style="margin-left: 10px">{{ scope.row.date}}</span>
                           </template>
                       </el-table-column>
                       <el-table-column
                           label="状态"
                           width="100">
                           <template slot-scope="scope">
                               <el-tag v-show="scope.row.state  =='未发布'" type="info">{{scope.row.state}}</el-tag>
                               <el-tag v-show="scope.row.state  =='已发布'" type="success">{{scope.row.state}}</el-tag>
                           </template>
                       </el-table-column>
                       <el-table-column
                           label="操作"
                           width="120">
                           <template slot-scope="scope">
                               <i style="font-size: 18px;margin: 0 5px;cursor: pointer" class="el-icon-edit-outline"></i>
                               <i style="font-size: 18px;margin: 0 5px;cursor: pointer"  class="el-icon-delete"></i>
                           </template>
                       </el-table-column>
                   </el-table>
               </div>
           </div>

           <div class="block paginationBox">
               <el-pagination
                   @current-change="handleCurrentChange"
                   :current-page="currentPage"
                   :page-sizes="1"
                   :page-size="100"
                   layout=" prev, pager, next, jumper"
                   :total="tableData.length">
               </el-pagination>
           </div>
       </div>
       <div v-if="!listPage">
           <div class="addPage">
               <div class="EditPage">

                   <h4 class="content-title"> <el-button @click="goListPage" class="el-icon-arrow-left">列表</el-button></h4>
                   <div class="edit_container">
                       <quill-editor v-model="content"
                                     ref="myQuillEditor"
                                     class="editer"
                                     :options="editorOption"
                                     @ready="onEditorReady($event)">
                       </quill-editor>
                   </div>
                   <div class="uploadBg">
                       <div>
                           <p>封面设置</p>
                           <el-upload
                               class="avatar-uploader"
                               :action="baseUrl + '/v1/addimg/shop'"
                               :show-file-list="false"
                               :on-success="handleAvatarSuccess"
                               :before-upload="beforeAvatarUpload">
                               <img v-if="imageUrl" :src="imageUrl" class="avatar">
                               <i v-else class="el-icon-plus avatar-uploader-icon"></i>
                           </el-upload>
                       </div>
                       <div>
                           <p>文章分类</p>
                           <el-select v-model="value" placeholder="选择分类">
                               <el-option
                                   v-for="item in options"
                                   :key="item.value"
                                   :label="item.label"
                                   :value="item.value">
                               </el-option>
                           </el-select>

                       </div>
                       <div class="glzb">
                           <p>关联主播</p>
                           <el-autocomplete
                               class="inline-input"
                               v-model="anchorOption"
                               :fetch-suggestions="querySearch"
                               placeholder="请输入内容"
                               :trigger-on-focus="false"
                               @select="handleSelect"
                           ></el-autocomplete>
                       </div>
                       <div class="submit_btn">
                           <el-button @click="submit">保存</el-button>
                           <el-button @click="mobilePreview()" >预览</el-button>
                           <el-button type="primary" @click="submit">提交</el-button>
                       </div>
                   </div>
               </div>
           </div>
       </div>
   </div>
</template>

<script>
    import headTop from '../components/headTop'
    import { quillEditor } from 'vue-quill-editor'
    import {baseUrl, baseImgPath} from '@/config/env'

    export default {
        data(){
            return {
                anchorOption:"",
                currentPage:1,
                zbName:"",
                listPage:true,
                imageUrl: '',
                baseUrl:"",
                content: '<h2> </h2>',
			    editorOption:
                    {},
                options: [{
                    value: '选项1',
                    label: '王者荣耀'
                }, {
                    value: '选项2',
                    label: '美女主播'
                }, {
                    value: '选项3',
                    label: '英雄联盟'
                }, {
                    value: '选项4',
                    label: '吃鸡秀场'
                }, {
                    value: '选项5',
                    label: '颜值'
                }],
                value: '',
                input:"",
                tableData: [{
                    date: '2018-05-20',
                    name: 'MSI',
                    state:"未发布",
                    content: {
                        imgUrl:"https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=1597246709,1256667767&fm=58&w=121&h=90&img.JPEG",
                        title:"msi之后各赛区选手排名，KZ输了但是kHan没输" ,
                    }
                }, {
                    date: '2018-05-20',
                    name: '微笑',
                    state:"已发布",
                    content: {
                        imgUrl:"https://imgsa.baidu.com/forum/w%3D580%3B/sign=b247fdbcb0315c6043956be7bd8ac913/71cf3bc79f3df8dc83b9de38c111728b46102896.jpg",
                        title:"微笑谈UZI夺冠",
                    }
                }, {
                    date: '2016-05-01',
                    name: '张大仙',
                    state:"已发布",
                    content: {
                        imgUrl:"https://ss0.baidu.com/6ONWsjip0QIZ8tyhnq/it/u=2057889765,690613900&fm=58&w=121&h=140&img.JPEG",
                        title:"4AM GODV 正在连接",
                    }
                }, {
                    date: '2016-05-03',
                    name: 'shinyRuo',
                    state:"已发布",
                    content: {
                        imgUrl:"https://ss0.baidu.com/73x1bjeh1BF3odCf/it/u=757669012,4021713166&fm=85&s=C440B946C052A5D610C44C3203005093",
                        title:"你把你闪现给我交了",
                    }
                }]
            }

        },
        created(){

        },
        watch:{

        },
    	components: {
    		quillEditor,
    	},
        computed: {
          	editor() {
	        	return this.$refs.myQuillEditor.quill
	      	}
        },

        methods: {
            //分页事件
            handleCurrentChange(val) {
                console.log(`当前页: ${val}`);
            },
            //
            handleAvatarSuccess(res, file) {
                this.imageUrl = URL.createObjectURL(file.raw);
            },
            beforeAvatarUpload(file) {
                const isJPG = file.type === 'image/jpeg';
                const isLt2M = file.size / 1024 / 1024 < 2;

                if (!isJPG) {
                    this.$message.error('上传图片只能是 JPG 格式!');
                }
                if (!isLt2M) {
                    this.$message.error('上传图片大小不能超过 2MB!');
                }
                return isJPG && isLt2M;
            },
		    onEditorReady(editor) {

		        console.log(this.content)
                console.log(editor)
		        console.log('editor ready!', editor)
		    },
		    submit(){
                var htFwb = this.content
                htFwb =  htFwb.replace(/\iframe/g, "video")
                console.log(htFwb)
                this.content = htFwb
                console.log(this.content);

                this.$message.success('提交成功！');
            },
            goAddPage(){
                this.listPage =false
            },
            goListPage(){
                this.listPage =true
            },
            mobilePreview(){
                this.$alert(this.content, '移动端预览', {
                    dangerouslyUseHTMLString: true
                });
            },
            querySearch(queryString, cb) {
                var restaurants = this.restaurants;
                var results = queryString ? restaurants.filter(this.createFilter(queryString)) : restaurants;
                // 调用 callback 返回建议列表的数据
                cb(results);
            },
            createFilter(queryString) {
                return (restaurant) => {
                    return (restaurant.value.toLowerCase().indexOf(queryString.toLowerCase()) === 0);
                };
            },
            loadAll() {
                return [
                    { "value": "骚白", "address": "长宁区新渔路144号" },
                    { "value": "冯提莫", "address": "上海市长宁区淞虹路661号" },
                    { "value": "卢本伟", "address": "上海市普陀区真北路988号创邑金沙谷6号楼113" },
                    { "value": "PDD", "address": "天山西路438号" },
                    { "value": "韦神", "address": "上海市长宁区金钟路968号1幢18号楼一层商铺18-101" },
                    { "value": "大哥", "address": "上海市长宁区金钟路633号" },
                    { "value": "微笑", "address": "上海市嘉定区曹安公路曹安路1685号" },
                    { "value": "德云色", "address": "上海市普陀区同普路1435号" },
                    { "value": "山泥若", "address": "上海市北翟路1444弄81号B幢-107" },
                    { "value": "大司马", "address": "上海市嘉定区新郁路817号" },
                    { "value": "UZI", "address": "嘉定区曹安路1611号" },
                    { "value": "123", "address": "嘉定区曹安公路2383弄55号" },
                    { "value": "888", "address": "嘉定区江桥镇曹安公路2409号1F，2383弄62号1F" },

                ];

            },
            //选择
            handleSelect(item) {
                console.log(item);
            }

        },

        mounted() {
            this.restaurants = this.loadAll();
        }
    }
</script>

<style lang="less">
	@import '../style/mixin';
	.edit_container{
		padding: 20px;
		margin-bottom: 60px;
	}
	.editer{
		height: 400px;
	}
	.submit_btn{
        margin-bottom: 10px;
		text-align: center;
	}
    .content-title{
        margin: 20px 0 0 20px;
    }
    .avatar-uploader .el-upload {
        border: 1px dashed #d9d9d9;
        border-radius: 6px;
        width: 100%;
        cursor: pointer;
        position: relative;
        overflow: hidden;
    }
    .avatar-uploader .el-upload:hover {
        border-color: #409EFF;
    }
    .avatar-uploader-icon {
        font-size: 28px;
        color: #8c939d;
        width: 100%;
        height: 100px;
        line-height: 100px;
        text-align: center;
    }
    .avatar-uploader{
        width: 75%;
    }
    .avatar {
        width: 100%;
        height: 100%;
        display: block;
    }
    .uploadBg{
        padding: 5px 20px;
        width: 100%;
        /*margin: 0 auto;*/
        height: 100%;
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    .uploadBg>div{
        width: 30%;
        margin: 0 10px;
        height: 100%;
        display: flex;
        align-items: center;
    }
    .uploadBg p{
        width: 80px;
        font-size: 14px;
    }
   .glzb .el-input{
       width: 75%;
   }
    .listPage:first-child{
        .el-input__inner{
            height: 30px;
            line-height: 30px;
            border-radius: 0;
        }
        .el-button{
            padding: 0 10px;
            border-radius: 0;
        }
    }
    .listPage{
        .cell{
            text-align: center;
        }

    }
    .tableContentBox{
        padding: 10px;
        width: 100%;
        font-size: 16px;
        display: flex;
        align-items: center;
    }
    .ql-snow .ql-tooltip[data-mode=video]::before {
        content: "视频地址:"!important;
    }
    .ql-snow .ql-tooltip[data-mode=link]::before {
        content: "链接地址:" !important;
    }
    .ql-snow .ql-tooltip.ql-editing a.ql-action::after {
        border-right: 0px;
        content: '保存' !important;
        padding-right: 0px;
    }
    .ql-snow .ql-tooltip {
        position: absolute;
        transform: translate(130px) !important;
    }
    .tableContentBox img{
        width: 180px;
        height: 100px;
    }
        .el-message-box{
            background-size: cover;
            background-repeat: no-repeat;
            width:375px;
            padding: 0 10px;
            img{
                width: 100%;
            }
          .el-message-box__content{
              height: auto;
              max-height: 500px;
              overflow: auto;
              .el-message-box__message{
                  p{
                      .ql-align-center{
                          text-align: center;
                      }
                  }
              }
              a
              {
                  color:#06c;
                  font-weight: initial;
              }
          }
        }


</style>
<style scoped>

</style>
