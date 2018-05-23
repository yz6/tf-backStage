<template>
    <div class="tfPageBox">
        <el-tabs v-model="activeName" @tab-click="">
            <el-tab-pane label="一级渠道" name="first">
                <div class="optionBox flex">
                    <div class="optionList">
                        <span>所在城市</span>
                        <span>&nbsp;&nbsp;</span>
                        <el-cascader
                            v-model="cityOptionMr"
                            :options="cityOption"
                            @active-item-change="handleItemChange"
                            :props="props"
                        ></el-cascader>
                        <span>&nbsp;&nbsp;&nbsp;</span>
                        <span>时间区间</span>
                        <span>&nbsp;&nbsp;</span>
                        <span class="timeZone">
                 <el-date-picker
                     v-model="timeInterval"
                     type="daterange"
                     :picker-options="pickerOptions"
                     align="center"
                     range-separator="至"
                     start-placeholder="开始日期"
                     end-placeholder="结束日期"
                     value-format="timestamp">
             </el-date-picker>
             </span>
                        <span>&nbsp;&nbsp;&nbsp;</span>
                        <el-input class="phoneInput"  style="width: 180px;" v-model="channelName" placeholder="渠道名"><el-button slot="append" icon="el-icon-search"></el-button></el-input>
                        <span>&nbsp;&nbsp;&nbsp;</span>
                        <el-button style="height: 30px; line-height: 0px" type="primary"  @click="dialogFormVisible = true">新增渠道</el-button>
                    </div>

                </div>
                <div class="overviewBox">
                    <el-card>

                              <div class="flex">
                                  <div class="overviewList">
                                      <p style="font-size: 16px">渠道总数 &nbsp;&nbsp;<span class="numType">137</span>&nbsp;&nbsp;单</p>
                                  </div>
                                  <div style="width: 100px"></div>
                                  <div class="overviewList">
                                      <p style="font-size: 16px">累计分成总金额 &nbsp;&nbsp;<span class="numType">7038</span>&nbsp;&nbsp;元</p>
                                  </div>
                              </div>

                    </el-card>

                </div>
                <div class="table">
                    <el-table
                        :data="tableData"
                        border
                        style="width: 100%;text-align: center">
                        <el-table-column
                            prop="channelName"
                            label="渠道名称">
                        </el-table-column>
                        <el-table-column
                            prop="city"
                            label="所在城市">
                        </el-table-column>
                        <el-table-column
                            prop="contacts"
                            label="联系人"
                            width="80">
                        </el-table-column>
                        <el-table-column
                            prop="phoneNum"
                            label="手机"
                            width="120">
                        </el-table-column>
                        <el-table-column
                            prop="price"
                            label="累计分成">
                        </el-table-column>
                        <el-table-column
                            prop="joinTime"
                            label="加入时间">
                        </el-table-column>
                        <el-table-column
                            label="操作">
                            <template slot-scope="scope">
                                <el-button
                                    @click.native.prevent=""
                                    type="text"
                                    size="small">
                                    编辑
                                </el-button>
                                <el-button
                                    @click.native.prevent="deleteRow(scope.$index, tableData)"
                                    type="text"
                                    size="small">
                                    移除
                                </el-button>
                            </template>
                        </el-table-column>
                    </el-table>
                </div>
            </el-tab-pane>
            <el-tab-pane label="二级渠道" name="second">
                <div class="optionBox flex">
                    <div class="optionList">
                        <span>所在城市</span>
                        <span>&nbsp;&nbsp;</span>
                        <el-cascader
                            v-model="cityOptionMr"
                            :options="cityOption"
                            @active-item-change="handleItemChange"
                            :props="props"
                        ></el-cascader>
                        <span>&nbsp;&nbsp;&nbsp;</span>
                        <span>一级渠道</span>
                        <span>&nbsp;&nbsp;</span>
                        <el-select v-model="channel" placeholder="请选择">
                            <el-option
                                v-for="item in channelOption"
                                :key="item.value"
                                :label="item.label"
                                :value="item.value">
                            </el-option>
                        </el-select>
                        <span>&nbsp;&nbsp;&nbsp;</span>
                        <span>时间区间</span>
                        <span>&nbsp;&nbsp;</span>
                        <span class="timeZone">
                 <el-date-picker
                     v-model="timeInterval"
                     type="daterange"
                     :picker-options="pickerOptions"
                     align="center"
                     range-separator="至"
                     start-placeholder="开始日期"
                     end-placeholder="结束日期"
                     value-format="timestamp">
             </el-date-picker>
             </span>
                        <span>&nbsp;&nbsp;&nbsp;</span>
                        <el-input class="phoneInput"  style="width: 180px;" v-model="channelName" placeholder="渠道名"><el-button slot="append" icon="el-icon-search"></el-button></el-input>
                        <span>&nbsp;&nbsp;&nbsp;</span>
                        <el-button style="height: 30px; line-height: 0px" type="primary"  @click="dialogFormVisible = true">新增渠道</el-button>
                    </div>

                </div>
                <div class="overviewBox">
                    <el-card>

                        <div class="flex">
                            <div class="overviewList">
                                <p style="font-size: 16px">渠道总数 &nbsp;&nbsp;<span class="numType">137</span>&nbsp;&nbsp;单</p>
                            </div>
                            <div style="width: 100px"></div>
                            <div class="overviewList">
                                <p style="font-size: 16px">累计分成总金额 &nbsp;&nbsp;<span class="numType">7038</span>&nbsp;&nbsp;元</p>
                            </div>
                        </div>

                    </el-card>

                </div>
                <div class="table">
                    <el-table
                        :data="tableData"
                        border
                        style="width: 100%;text-align: center">
                        <el-table-column
                            prop="channelName"
                            label="渠道名称">
                        </el-table-column>
                        <el-table-column
                            prop="city"
                            label="所在城市">
                        </el-table-column>
                        <el-table-column
                            prop="ssqd"
                            label="所属渠道">
                        </el-table-column>
                        <el-table-column
                            prop="contacts"
                            label="联系人"
                            width="80">
                        </el-table-column>
                        <el-table-column
                            prop="phoneNum"
                            label="手机"
                            width="120">
                        </el-table-column>
                        <el-table-column
                            prop="price"
                            label="累计分成">
                        </el-table-column>
                        <el-table-column
                            prop="joinTime"
                            label="加入时间">
                        </el-table-column>
                        <el-table-column
                            label="操作">
                            <template slot-scope="scope">
                                <el-button
                                    @click.native.prevent=""
                                    type="text"
                                    size="small">
                                    编辑
                                </el-button>
                                <el-button
                                    @click.native.prevent="deleteRow(scope.$index, tableData)"
                                    type="text"
                                    size="small">
                                    移除
                                </el-button>
                            </template>
                        </el-table-column>
                    </el-table>
                </div>
            </el-tab-pane>
        </el-tabs>
        <!--编辑&新增表单-->
        <div class="formBox">
            <el-dialog width="500px" :visible.sync="dialogFormVisible">
                <el-form label-position="left" :model="form">
                    <el-form-item label="所在城市" :label-width="formLabelWidth">
                        <el-cascader
                            v-model="form.city"
                            :options="cityOption"
                            @active-item-change="handleItemChange"
                            :props="props"
                        ></el-cascader>
                    </el-form-item>
                    <el-form-item label="渠道等级" :label-width="formLabelWidth">
                        <el-select v-model="form.region" placeholder="请选择活动区域">
                            <el-option label="一级渠道" value="level1"></el-option>
                            <el-option label="二级渠道" value="level2"></el-option>
                        </el-select>
                    </el-form-item>
                    <el-form-item label="渠道名称" :label-width="formLabelWidth">
                        <el-input auto-complete="off" v-model="form.name"></el-input>
                    </el-form-item>
                    <el-form-item label="联系人" :label-width="formLabelWidth">
                        <el-input v-model="form.contacts"></el-input>
                    </el-form-item>
                    <el-form-item label="手机" :label-width="formLabelWidth">
                        <el-input v-model="form.phoneNum"></el-input>
                    </el-form-item>
                </el-form>
                <div slot="footer" class="dialog-footer">
                    <el-button @click="dialogFormVisible = false">取 消</el-button>
                    <el-button type="primary" @click="dialogFormVisible = false">确 定</el-button>
                </div>
            </el-dialog>
        </div>

    </div>
</template>

<script>
    export default {
        name: "channelManagement",
        data(){
            return{
                channelName:"",
                activeName:"first",
                //渠道
                channel:"",
                //手机
                phoneNum:"",
                //支付状态
                payState:"",
                //时间区间
                timeInterval:"",
                //tableData
                tableData:[{
                    channelName: '义乌外贸有限公司',
                    city:"义乌市",
                    contacts:"张凯",
                    doneTime: '2018/05/23 12:00',
                    price: '140',
                    joinTime:"2018/05/12",
                    phoneNum:"18017265821",
                    cityNum:"义乌城时代理 : 2元",
                    qd1:"义乌外贸：2元",
                    qd2:"18012467899:1.2元",
                    yh1:"18015554678:0.4元",
                    yh2:"15617249199:0.4元",
                    ssqd:"义务外贸"
                }, {
                    date: '2016-05-04',
                    name: '王小虎',
                    address: '上海市普陀区金沙江路 1517 弄'
                }, {
                    date: '2016-05-01',
                    name: '王小虎',
                    address: '上海市普陀区金沙江路 1519 弄'
                }, {
                    date: '2016-05-03',
                    name: '王小虎',
                    address: '上海市普陀区金沙江路 1516 弄'
                }],
                payStateOption:[{
                    value:"1",
                    label:"已支付"
                },{
                    value:"2",
                    label:"未支付"
                }],
                channelOption:[{
                    value: '1',
                    label: '义乌外贸'
                }],
                cityOptionMr:["js","yw"],
                cityOption: [{
                    value:"js",
                    label: '江苏',
                    cities: [
                        {
                            value:"yw",
                            label: '义乌'
                        }
                    ]
                }, {
                    label: '浙江',
                    cities: []
                }],
                props: {
                    value: 'label',
                    children: 'cities'
                },
                pickerOptions: {
                    shortcuts: [{
                        text: '最近一周',
                        onClick(picker) {
                            const end = new Date();
                            const start = new Date();
                            start.setTime(start.getTime() - 3600 * 1000 * 24 * 7);
                            picker.$emit('pick', [start, end]);
                        }
                    }, {
                        text: '最近一个月',
                        onClick(picker) {
                            const end = new Date();
                            const start = new Date();
                            start.setTime(start.getTime() - 3600 * 1000 * 24 * 30);
                            picker.$emit('pick', [start, end]);
                        }
                    }, {
                        text: '最近三个月',
                        onClick(picker) {
                            const end = new Date();
                            const start = new Date();
                            start.setTime(start.getTime() - 3600 * 1000 * 24 * 90);
                            picker.$emit('pick', [start, end]);
                        }
                    }]
                },
                dialogFormVisible: false,
                //新增 编辑
                form: {
                    city:"",
                    name: '',
                    level:"",
                    phoneNum:"",
                    delivery: false,
                    type: [],
                    contacts:"",
                },
                formLabelWidth: '100px'
            }
        },
        methods:{
            deleteRow(index, rows) {
                rows.splice(index, 1);
            },
        }
    }
</script>

<style lang="less">
    .tfPageBox{
        font-size: 14px;
        padding: 20px;
    }
    .flex{
        display: flex;
    }
    .optionBox{
        width: 100%;
        display: flex;
        align-items: center;
        margin-bottom: 10px;
    }
    .optionList{
        display: flex;
        align-items: center;
        .el-input.is-active .el-input__inner, .el-input__inner:focus {
            border-color:#dcdfe6;
            outline: 0;
        }
        .el-select .el-input.is-focus .el-input__inner {
            border-color:#dcdfe6;
        }
        .el-input__inner{
            width: 120px;
            height: 30px;
            line-height: 30px;
        }
        .phoneInput{
            .el-input__inner{
                width: 150px;

            }
        }
        .timeZone{
            .el-input__inner{
                width: 240px;

            }
            .el-date-editor .el-range__icon{
                line-height: 0;
            }
            .el-date-editor .el-range-separator{
                line-height: 20px;
            }
            .el-date-editor .el-range__close-icon{
                line-height: 20px;
            }
        }

    }
    .overviewBox{
        margin-bottom: 20px;
    }
    .overviewList{
        position: relative;
        text-align: center;
        p:first-child{
            height: 60%;
            margin-bottom: 10px;
        }
        p:last-child{
            height: 40%;
            color: #666;
            font-size: 12px;
        }

    }
    .numType{
        font: 18px Medium;
        color: #409EFF;
    }
    .formBox .el-input, .demo-box.demo-dialog .el-select{
        width: 300px;
    }
    .formBox {
        .el-form-item__label{
            font-size: 16px;
        }

    }
</style>
