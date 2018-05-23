<template>
    <div class="tfPageBox">
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
             <span>&nbsp;&nbsp;&nbsp;&nbsp;</span>
             <span>所属渠道</span>
             <span>&nbsp;&nbsp;</span>
             <el-select v-model="channel" placeholder="请选择">
                 <el-option
                     v-for="item in channelOption"
                     :key="item.value"
                     :label="item.label"
                     :value="item.value">
                 </el-option>
             </el-select>
             <span>&nbsp;&nbsp;&nbsp;&nbsp;</span>
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
             <span>&nbsp;&nbsp;&nbsp;&nbsp;</span>
             <el-input class="phoneInput"  style="width: 180px;" v-model="phoneNum" placeholder="输入手机号"><el-button slot="append" icon="el-icon-search"></el-button></el-input>
         </div>

        </div>
        <div class="overviewBox">
            <el-card>
                <el-row  type="flex"  justify="space-around">
                    <el-col  >
                        <div class="overviewList">
                        <p>订单总数</p>
                        <p><span class="numType">137</span>&nbsp;&nbsp;单</p>
                    </div>
                    </el-col>
                    <el-col >
                        <div class="overviewList">
                            <p>实际总金额</p>
                            <p><span class="numType">7038</span>&nbsp;&nbsp;元</p>
                        </div>
                    </el-col>
                    <el-col >
                        <div class="overviewList">
                            <p>城市代理总分成</p>
                            <p><span class="numType">3226</span>&nbsp;&nbsp;元</p>
                        </div>
                    </el-col>
                    <el-col  >
                        <div class="overviewList">
                            <p>一级渠道总分成</p>
                            <p><span class="numType">1050</span>&nbsp;&nbsp;元</p>
                        </div>
                    </el-col>
                    <el-col >
                        <div class="overviewList">
                            <p>二级渠道总分成</p>
                            <p><span class="numType">2073</span>&nbsp;&nbsp;元</p>
                        </div>
                    </el-col>
                    <el-col>
                        <div class="overviewList">
                            <p>一级用户总分成</p>
                            <p><span class="numType">566</span>&nbsp;&nbsp;元</p>
                        </div>
                    </el-col>
                    <el-col>
                        <div class="overviewList">
                            <p>二级用户总分成</p>
                            <p><span class="numType">125</span>&nbsp;&nbsp;元</p>
                        </div>
                    </el-col>
                </el-row>
            </el-card>

        </div>
        <div class="table">
            <el-table
                :data="tableData"
                border
                style="width: 100%;text-align: center">
                <el-table-column
                    prop="orderId"
                    label="订单号"
                    width="120">
                </el-table-column>
                <el-table-column
                    prop="doneTime"
                    label="结算时间">
                </el-table-column>
                <el-table-column
                    prop="price"
                    label="实收金额"
                    width="80">
                </el-table-column>
                <el-table-column
                    prop="phoneNum"
                    label="付款用户"
                    width="120">
                </el-table-column>
                <el-table-column
                    prop="qd1"
                    label="一级渠道分成">
                </el-table-column>
                <el-table-column
                    prop="qd2"
                    label="二级渠道分成">
                </el-table-column>
                <el-table-column
                    prop="yh1"
                    label="一级用户分成">
                </el-table-column>
                <el-table-column
                    prop="yh2"
                    label="二级用户分成">
                </el-table-column>
            </el-table>
        </div>
    </div>
</template>

<script>
    export default {
        name: "divide-into",
        data(){
            return{
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
                    orderId: '20180523008',
                    doneTime: '2018/05/23 12:00',
                    price: '40',
                    phoneNum:"18017265821",
                    cityNum:"义乌城时代理 : 2元",
                    qd1:"义乌外贸：2元",
                    qd2:"18012467899:1.2元",
                    yh1:"18015554678:0.4元",
                    yh2:"15617249199:0.4元"

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
                }
            }
        },
        methods:{
            handleItemChange(val) {
                console.log('active item:', val);
                setTimeout(_ => {
                    if (val.indexOf('江苏') > -1 && !this.cityOption[0].cities.length) {
                        this.cityOption[0].cities = [{
                            value:"nj",
                            label: '南京'
                        }];
                    } else if (val.indexOf('浙江') > -1 && !this.cityOption[1].cities.length) {
                        this.cityOption[1].cities = [{
                            value:"yw",
                            label: '义乌'
                        }];
                    }
                }, 300);
            }
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
                width: 260px;

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
        height: 80%;
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
        font: 16px Medium;
        color: #409EFF;
    }
</style>
