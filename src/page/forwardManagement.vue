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
                <span>&nbsp;&nbsp;&nbsp;</span>
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
                <span>&nbsp;&nbsp;&nbsp;</span>
                <span>支付状态</span>
                <span>&nbsp;&nbsp;</span>
                <el-select v-model="payState" placeholder="请选择">
                    <el-option
                        v-for="item in payStateOption"
                        :key="item.value"
                        :label="item.label"
                        :value="item.value">
                    </el-option>
                </el-select>
                <span>&nbsp;&nbsp;&nbsp;</span>
                <el-input class="phoneInput"  style="width: 180px;" v-model="channelName" placeholder="渠道名"><el-button slot="append" icon="el-icon-search"></el-button></el-input>

            </div>

        </div>
        <div class="optionList">
            <span>时间区间</span>
            <span>&nbsp;</span>
            <span class="timeZone">
                 <el-date-picker
                     v-model="timeInterval"
                     type="daterange"
                     :picker-options="pickerOptions"
                     align="left"
                     range-separator="至"
                     start-placeholder="开始日期"
                     end-placeholder="结束日期"
                     value-format="timestamp">
             </el-date-picker>
                  </span>
        </div>
    </div>
</template>

<script>
    export default {
        name: "forward-management",
        data(){
            return{
                channel:"",
                //支付状态
                payState:"",
                payStateOption:[{
                    value:"1",
                    label:"已支付"
                },{
                    value:"2",
                    label:"未支付"
                }],
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
                //时间区间
                timeInterval:"",
                channelOption:[{
                    value: '1',
                    label: '义乌外贸'
                }],
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

    }</style>
