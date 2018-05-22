<template>
    <div class="bodyBox">
        <el-tabs  v-model="activeName" @tab-click="handleClick">
            <el-tab-pane  label="上传管理" name="first">
                <div>
                    <div v-if="listPage">
                        <div class="listPage">
                            <p style="float: left" class="content-title1">视频(共4条)</p>
                            <div style="float: right;padding: 0px 10px 0 0;display: flex;margin-bottom: 20px;width: 350px">
                                <el-input
                                    placeholder="主播名"
                                    v-model="zbName">
                                </el-input>
                                <el-button  class="el-icon-search" style="width: 60px"></el-button>
                                <el-button @click="goAddPage" class="addWz" style="margin: 0 15px;width: 180px;" type="primary" >&nbsp; 新建视频 &nbsp;</el-button>
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
                                :page-size="100"
                                layout=" prev, pager, next, jumper"
                                :total="tableData.length">
                            </el-pagination>
                        </div>

                    </div>
                    <div v-if="!listPage">
                        <div class="addPage addvideoPage">
                            <div class="EditPage">

                                <h4  style="padding-left: 20px;"> <el-button @click="goListPage" class="el-icon-arrow-left">列表</el-button></h4>
                                <div class="uploadBg1">
                                    <div>
                                        <div style="width: 60px">
                                            <p>视频链接</p>
                                        </div>
                                        <div class="ljBox" style="margin-left: 20px">
                                            <el-input v-model="videoUrl" placeholder=""></el-input>

                                        </div>
                                    </div>
                                    <div>
                                        <div style="width:  60px">
                                            <p>标题</p>
                                        </div>
                                        <div class="ljBox" style="margin-left: 20px">
                                            <el-input v-model="videoTitle" placeholder=""></el-input>

                                        </div>
                                    </div>
                                    <div>
                                     <div style="width:  60px">
                                         <p>封面</p>
                                     </div>
                                        <div style="margin-left: 20px">
                                            <el-upload
                                                class="avatar-uploader1"
                                                :action="baseUrl + '/v1/addimg/shop'"
                                                :show-file-list="false"
                                                :on-success="handleAvatarSuccess"
                                                :before-upload="beforeAvatarUpload">
                                                <img v-if="imageUrl" :src="imageUrl" class="avatar">
                                                <i v-else class="el-icon-plus avatar-uploader-icon"></i>
                                            </el-upload>

                                        </div >
                                        <div style="margin-left: 20px">
                                            <p style="color: #ccc">(默认为视频第一帧)</p>
                                        </div>
                                    </div>

                                    <div >
                                        <div style="width: 60px;">
                                            <p>标签</p>
                                        </div>
                                        <div style="margin-left: 20px">
                                            <el-tag
                                                :key="tag"
                                                v-for="tag in dynamicTags"
                                                closable
                                                :disable-transitions="false"
                                                @close="handleClose(tag)">
                                                {{tag}}
                                            </el-tag>
                                            <el-input
                                                class="input-new-tag"
                                                v-if="inputVisible"
                                                v-model="inputValue"
                                                ref="saveTagInput"
                                                size="small"
                                                @keyup.enter.native="handleInputConfirm"
                                                @blur="handleInputConfirm"
                                            >
                                            </el-input>
                                            <el-button v-show="dynamicTags.length<3"  v-else class="button-new-tag" size="small" @click="showInput"><i class="el-icon-circle-plus"></i>&nbsp;新建标签</el-button>
                                            <span v-if="dynamicTags.length<3" style="color: #ccc;font-size: 14px">(最多3个)</span>

                                        </div>

                                    </div>
                                    <div>
                                        <div style="width: 60px;">
                                            <p>简介</p>
                                        </div>
                                        <div style="margin-left: 20px;width: 400px;">
                                            <el-input
                                                type="textarea"
                                                :autosize="{ minRows: 2, maxRows: 4}"
                                                placeholder="请输入内容"
                                                v-model="videoDetail">
                                            </el-input>
                                        </div>
                                    </div>
                                    <div>
                                        <div style="width:  60px">
                                            <p>关联主播</p>
                                        </div>
                                        <div class="ljBox" style="margin-left: 20px">
                                            <el-input v-model="glzb" placeholder=""></el-input>

                                        </div>
                                    </div>
                                    <div class="submit_btn">
                                        <el-button @click="submit">保存</el-button>
                                        <el-button type="primary" @click="submit">提交</el-button>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </el-tab-pane>
            <el-tab-pane label="下载管理" name="second">
                <div class="downLoad">
                    <div class="downLoadTop">
                      <div>
                          <span>直播平台</span>
                          <span>&nbsp;</span>
                          <el-select class="paltFrom" v-model="livePlatform" placeholder="请选择">
                              <el-option
                                  v-for="item in platFromOption"
                                  :key="item.value"
                                  :label="item.label"
                                  :value="item.value">
                              </el-option>
                          </el-select>
                      </div>
                    <div>
                        <span>&nbsp;&nbsp;&nbsp;</span>
                        <span>直播类型</span>
                        <span>&nbsp;</span>
                        <el-select class="styleInput"  v-model="styleOption" placeholder="请选择">
                            <el-option
                                v-for="item in options"
                                :key="item.value"
                                :label="item.label"
                                :value="item.value">
                            </el-option>
                        </el-select>
                    </div>
                    <div>
                        <span>&nbsp;&nbsp;&nbsp;</span>
                        <span>直播日期</span>
                        <span>&nbsp;</span>
                        <el-date-picker
                            v-model="liveDate"
                            style="width: 150px"
                            align="right"
                            type="date"
                            placeholder="选择日期"
                            :picker-options="pickerOptions">
                        </el-date-picker>
                    </div>
                        <div style="display: flex;align-items: center">
                            <span>&nbsp;&nbsp;&nbsp;</span>
                            <span>&nbsp;&nbsp;&nbsp;</span>
                            <el-input class="anchorName" style="width: 150px;" v-model="anchorName" placeholder="主播名称"><el-button slot="append" icon="el-icon-search"></el-button></el-input>

                        </div>
                    </div>
                    <div class="downTable">
                        <el-table
                            :data="downLoadData"
                            border
                            style="width: 100%">
                            <el-table-column
                                prop="date"
                                label="主播名称"
                                width="180">
                            </el-table-column>
                            <el-table-column
                                prop="name"
                                label="直播平台"
                                width="180">
                            </el-table-column>
                            <el-table-column
                                prop="address"
                                label="直播类型">
                            </el-table-column>
                            <el-table-column
                                prop="address"
                                label="直播日期">
                            </el-table-column>
                            <el-table-column
                                prop="address"
                                label="直播时长">
                            </el-table-column>
                            <el-table-column
                                prop="address"
                                label="更新时间">
                            </el-table-column>
                            <el-table-column
                                label="操作"
                            width="220">
                                <template slot-scope="scope">
                                    <el-button
                                        size="mini"
                                        type="text"
                                        @click="">查看弹幕趋势</el-button>
                                    <el-button
                                        size="mini"
                                        type="text"
                                        @click="">下载视频</el-button>
                                </template>
                            </el-table-column>
                        </el-table>
                    </div>
                </div>
            </el-tab-pane>
        </el-tabs>
    </div>
</template>

<script>
    import { quillEditor } from 'vue-quill-editor'
    import {baseUrl, baseImgPath} from '@/config/env'
    export default {
        name: "video",
        data() {
            return {
                dynamicTags: [],
                inputVisible: false,
                inputValue: '',
                anchorName:"",
                currentPage:1,
                activeName: 'first',
                zbName:"",
                listPage:true,
                imageUrl: '',
                baseUrl:"",
                videoLabel:"",
                videoUrl:"",
                videoTitle:"",
                videoDetail:"",
                glzb:"",
                livePlatform:"0",
                styleOption:"选项0",
                liveDate:"",
                downLoadData:[{

                }],
                editorOption: {

                },
                dateOption:[{
                    value:"2018-05-21"
                },{
                    value:"2018-05-22"
                },{
                    value:"2018-05-23"
                }],
                platFromOption:[{
                    value:"0",
                    label:"全部"
                    },{
                    value:"1",
                    label:"熊猫"
                },{
                    value:"2",
                    label:"斗鱼"
                },{
                    value:"3",
                    label:"虎牙"
                },{
                    value:"4",
                    label:"龙珠"
                },{
                    value:"5",
                    label:"战旗"
                },{
                    value:"6",
                    label:"企鹅"
                },{
                    value:"7",
                    label:"YY"
                }],
                pickerOptions:{
                shortcuts: [{
                    text: '今天',
                    onClick(picker) {
                        picker.$emit('pick', new Date());
                    }
                }, {
                    text: '昨天',
                    onClick(picker) {
                        const date = new Date();
                        date.setTime(date.getTime() - 3600 * 1000 * 24);
                        picker.$emit('pick', date);
                    }
                }, {
                    text: '一周前',
                    onClick(picker) {
                        const date = new Date();
                        date.setTime(date.getTime() - 3600 * 1000 * 24 * 7);
                        picker.$emit('pick', date);
                    }
                }]},
                options: [{
                    value: '选项0',
                    label: '全部类型'
                },{
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
                    date: '2016-05-02',
                    name: '骚白',
                    state:"已发布",
                    content: {
                        imgUrl:"https://ss0.baidu.com/73F1bjeh1BF3odCf/it/u=1501016046,831139799&fm=85&s=E8925F9C48B14B820A3634F703005030",
                        title:"骚白两亿身价空降斗鱼",
                    }
                }, {
                    date: '2016-05-04',
                    name: '德云色',
                    state:"已发布",
                    content: {
                        imgUrl:"https://ss0.baidu.com/73t1bjeh1BF3odCf/it/u=2034957386,672452555&fm=85&s=A97A6887401A9BD6163729000300E0C8",
                        title:"笑笑西卡谈S8决赛:'中国队最有希望的一年'",
                    }
                }, {
                    date: '2016-05-01',
                    name: '张大仙',
                    state:"已发布",
                    content: {
                        imgUrl:"https://ss0.baidu.com/73t1bjeh1BF3odCf/it/u=2485988117,1322660130&fm=85&s=AD7241871C0320DE5925EC9A0300C016",
                        title:"指法芬芳张大仙",
                    }
                }, {
                    date: '2016-05-03',
                    name: '冯提莫',
                    state:"已发布",
                    content: {
                        imgUrl:"https://ss0.baidu.com/73F1bjeh1BF3odCf/it/u=2850841584,1785072203&fm=85&s=1135CF340F274717B654E5CB0300A0B4",
                        title:"天王盖地虎,提莫一米五",
                    }
                }]
            };
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
            //tab切换时调用
            handleClick(tab, event) {
                console.log(tab, event);
            },
            //标签
            handleClose(tag) {
                this.dynamicTags.splice(this.dynamicTags.indexOf(tag), 1);
            },

            showInput() {
                this.inputVisible = true;
                this.$nextTick(_ => {
                    this.$refs.saveTagInput.$refs.input.focus();
                });
            },

            handleInputConfirm() {
                let inputValue = this.inputValue;
                if (inputValue) {
                    this.dynamicTags.push(inputValue);
                }
                this.inputVisible = false;
                this.inputValue = '';
            }
        }
    }
</script>

<style lang="less">
    @import '../style/mixin';
    .el-tag + .el-tag {
        margin-left: 10px;
    }
    .button-new-tag {
        height: 32px;
        line-height: 30px;
        padding-top: 0;
        padding-bottom: 0;
    }
    .input-new-tag {
        width: 90px;
        vertical-align: bottom;
    }
    .bodyBox{
        margin-top: 10px;
        .el-tabs__item{
            font-size: 16px;
        }
    }
    .bodyBox .el-tabs__header{
        margin:10px 20px;
    }
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
    .uploadBg1 >.submit_btn{
        display: flex;
        justify-content: center;
    }
    .ql-editor h2:first-child{
        border-bottom: 1px solid #eee;
    }

    .avatar-uploader1 .el-upload {
        border: 1px dashed #d9d9d9;
        border-radius: 6px;
        width: 100%;
        cursor: pointer;
        position: relative;
        overflow: hidden;
    }
    .avatar-uploader1 .el-upload:hover {
        border-color: #409EFF;
    }
    .avatar-uploader1-icon {
        font-size: 28px;
        color: #8c939d;
        width: 200px;
        height: 100px;
        line-height: 100px;
        text-align: center;
    }
    .avatar-uploader1 .el-upload{
        line-height: 100px;
        height: 100px;
    }
    .avatar-uploader1{
        height: 100px;
        width:200px;
    }
    .avatar {
        width: 100%;
        height: 100%;
        display: block;
    }
    .uploadBg1{
        padding: 5px;
        width: 55%;
        margin: 0 auto;
    }
    .uploadBg1>div{
        text-align: left;
        width: 100%;
        margin: 20px 10px;
        height: 100%;
        display: flex;
        align-items: center;
    }
    .uploadBg1 p{
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
    .content-title1{
        margin-left: 20px;
        margin-top: 5px;
        padding-left: 20px;
    }
    .ljBox{
        width: 400px;
    }
    .el-textarea__inner{
        width: 100%;
    }
    .downLoad{
        padding: 5px 20px;
    }
    .downLoadTop{
        margin-top: 10px;
        font-size: 14px;
        display: flex;
        align-items: center;
    }
    .downLoadTop .el-input__inner{
        height: 30px;
    }
    .paltFrom .el-input{
        width: 120px;
    }
    .el-select .el-input__inner:focus{
        border: 1px solid #dcdfe6;
    }
    .styleInput .el-input{
        width: 120px;
    }
    .downLoadTop .el-input__icon{
        line-height: 30px;
    }
    .downLoadTop .anchorName{
        width: 120px;
    }
    .el-input-group__append{
        padding: 0 10px;
    }
    .downTable{
        padding-top:20px ;
    }
</style>
