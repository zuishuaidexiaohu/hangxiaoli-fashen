<template>
    <div class="main" @click="stopPreventEvent">
        <div class="suggestpanle">
            <div class="titlepanle">
                <text class="text1">审批意见</text>
                <div @click="onCloseDialog" class="onclosedialog">
                    <image style='width:24px; height:24px;' :src="iconOne"></image>
                </div>
            </div>
            <div class="suggestchoice">
                <div class="choiceitem" @click="clickChoice(item,index)" v-for="(item,index) in buttonArrs" :key="index">
                    <image style='width:32px; height:32px;margin-right:16px;' :src="iconThree" v-if="isChoice===index"></image>                    
                    <image style='width:32px; height:32px;margin-right:16px;' :src="iconTwo" v-else></image>
                    <text class="text2">{{item.title}}</text>
                </div>
            </div>
            <div class="onsubmit" @click="onSubmit">
                <text class="text3">确定</text>
            </div>
        </div>
    </div>
</template>

<style scoped>
    .main{
        position:fixed;
        top: 0;
        right: 0;
        left: 0;
        bottom: 0;
        background-color: rgba(0,0,0,0.30);
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }
    .suggestpanle{
        width: 560px;
        flex-direction: column;
        justify-content: flex-start;
        align-items: center;
        background-color: #FFF;
    }
    .titlepanle{
        width: 560px;
        height: 80px;
        border-bottom-width: 2px;
        border-bottom-style: solid;
        border-bottom-color: #F5F5F9;
        padding-left: 32px;
        padding-right: 32px;
        flex-direction: row;
        justify-content: space-between;
        align-items: center;
    }
    .onclosedialog{
        width: 80px;
        height: 80px;
        flex-direction: row;
        justify-content: flex-end;
        align-items: center;
    }
    .text1{
        font-family: PingFangSC-Medium;
        font-size: 28px;
        color: #333333;
    }
    .text2{
        font-family: PingFangSC-Regular;
        font-size: 28px;
        color: #333333;
    }
    .text3{
        font-family: PingFangSC-Regular;
        font-size: 34px;
        color: #00A4EA;
    }
    .suggestchoice{
        width: 560px;
        /* flex-direction: row; */
        padding-left: 32px;
        /* padding-right: 32px; */
        /* justify-content: space-between; */
        /* align-items: center; */
        /* flex-wrap: wrap; */
    }
    .choiceitem{
        width: 248px;
        height: 80px;
        flex-direction: row;
        justify-content: flex-start;
        align-items: center;
    }
    /* .inputmenoy{
        width: 560px;
        height: 100px;
        flex-direction: row;
        padding-left: 32px;
        padding-right: 32px;
        justify-content: space-between;
        align-items: center;
    }
    .inputtext{
        width: 324px;
        height: 80px;
        text-align: right;
        background-color: #F8F8F8;
        border-radius: 4px;
        padding-right: 10px;
        padding-left: 10px;
    }
    .textarea{
        width: 496px;
        height: 262px;
        margin-top: 20px;
        margin-bottom: 40px;
        padding: 10px;
        background-color: #f8f8f8;
        border-radius: 4px;
        font-family: PingFangSC-Regular;
        font-size: 24px;
        color: #333333;
    } */
    .onsubmit{
        width: 560px;
        height: 100px;
        border-top-width: 2px;
        border-top-style: solid;
        border-top-color: #F5F5F9;
        flex-direction: row;
        justify-content: center;
        align-items: center;
    }
</style>

<script>
    import { weexRequest } from '@/utils/weexAxios.js'
    import {navigatorPushEvent,navigatorPopEvent} from '@/utils/navigator'
    const modal = weex.requireModule('modal');
    export default {
        name:'suggestDialog',
        data() {
            return {
                iconOne:'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAYAAADgdz34AAAAAXNSR0IArs4c6QAAAERlWElmTU0AKgAAAAgAAYdpAAQAAAABAAAAGgAAAAAAA6ABAAMAAAABAAEAAKACAAQAAAABAAAAGKADAAQAAAABAAAAGAAAAADiNXWtAAABbklEQVRIDaWWMW7DMAxFLV0jyHl6gQJZWqSLPfcome2lQbsU6K2CXkPu/4YZyJEo0SoBwYpIvi/RTBQ3juPTPM9T13Ue9t73/Q/mzTZN03MI4QJAcM71foUfsXCA4xsBL6105pJBFsaRbI8Jh5hHwLVFBJV4ZS5AG95SFiwGUWDAXhFuCLv9YG7ECSy5Z80xOcPRJLKW5XHnhJ/JdqJYCfySuPhpybkLMNGSIALW2I2AVcQKJy8RqInQn+kWqXlSyqwAIWy7XGfQB3vsFr7QBM5AVYBOpRR0iak7l4CiAIMKIlU48+Oj8nNiKFN1E0lStFBMVt5DlN5VT6EKKHD5tscnL4pkBTT4+pPyvzYtwaUVlRefPcnmBBa4vACryF1gD3yPyCLQAreKOByVdyivOXNnCFyehQ2eeHvxgm6GU2QYhk9c8G+YShtzeWETHC9mO4HRNVNEgudfCyTfMH7R5ydpxRow56cIGWRh3Mj+AyjeSXqTerQyAAAAAElFTkSuQmCC',
                iconTwo:'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAYAAABzenr0AAAAAXNSR0IArs4c6QAAAERlWElmTU0AKgAAAAgAAYdpAAQAAAABAAAAGgAAAAAAA6ABAAMAAAABAAEAAKACAAQAAAABAAAAIKADAAQAAAABAAAAIAAAAACshmLzAAADDklEQVRYCcWXMUhbQRjHkxeSDIIEurRqBqFQcCg4FOwgpUOXFKRSyVCHEIK4d23pUNrVPQQJDjqIpSLUpUMRhwoOGQpCoeCQarsUxOCQhCT9/x534UxjYxtfPLh8793d9///77vc3ffCoUuW9fX12MnJyUMNn1GdUB1ptVojuIfD4WMZ6oHqViKR+JROp2t67lnCvUYUi8WbtVrtlcbNqw73Gm/6T2VXY7HY62w2+/NvPhcK2N7ejpfL5Rea3XPNdMiAlPS+qfdd2SPNlFmHFBmiMaq2adknapqkXe9nel9KJpNvU6lUlbbO0lUAs67X6+/lPIWDgDYikcjLXC73tROg2/vy8vKdRqPxRv5zxn8vGo3OdovGHwIKhcJdOX5QHRPxoeqzhYWFvW5EvdqENSWcNdVx4XyXTS0uLn5x/c4JMDPfN+Q78Xj8aSaT+eU6/OvzysrKjWq1+k6YDxChSNxzI+FZQNbchJ2Z78jhUb/kYIMBlsEcgwMuy9sWwB9OA6c08JCZK1R1O6hfCxaYYMMBl8X0l8CE/ps6hzzPu/+/a25BL7L8J5rN5mcJOdNS3GYp/AiwzyFXx0ZQ5IgCGw64zNkS8jjh1MchE2KrYYMsDsc83J45XjnhSpfd5/0INBwlYQzDzRJwtnPYbGIHURyuGQRwsYS0LruDIO/gmkCAvdGOBiVAEbBcI55m7guwF8sgRFguuP1tCGmlUjl3LAcpxHIpEi1PP/6VqtvrVpCkLrbD9YMI+AIUjlF3UJDPDtcxAkij2IbTQZK62A7XAQK26JQqMpmBFIdryyOBFCs53CSZTNAKDAcp2yncnsleVyEmjQpagMOxCre/DcletS4kkHNcmUGJMCnaHFxwwuML4F4W+RINsmukUTxfZQETbDDhsmmZL4BGUmcp21PnODlcPp+P0n4VBSyTF47DAZfFbQsgbyd11gCyVxLIj1cRCTDAMpgkpbPuN0JbAIoIiwY/tiKker+f/wS+YFhy2ZQNvY1A1/P/Wj9MrLJr/TSzIrBE41o+Tl0RPAf1ef4btCjwEp3hpBQAAAAASUVORK5CYII=',
                iconThree:'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAYAAABzenr0AAAAAXNSR0IArs4c6QAAAERlWElmTU0AKgAAAAgAAYdpAAQAAAABAAAAGgAAAAAAA6ABAAMAAAABAAEAAKACAAQAAAABAAAAIKADAAQAAAABAAAAIAAAAACshmLzAAAC50lEQVRYCcVXz2sTQRT+JptufyTUGhMoQQShpx68aKGn4g8UWkEQpJd6F72IXgUPFsGTQkH9C9pDLwVBb1IPPQSkJyF40JNSxZQqmjSYGNf3ZnaazTq7Gzeb+KDd7Px43zdv3rz5VqBbW3dsNPfOwGldAsQ0BIqAQ39sYgcOdui9DGE9w1BuE4uiofrC/4vwbupdr06iWb9LAEsEMB45Xg4Q34ngKoZG72Ex+zlsTjCBF84wvu7egePcJgeZMCchfTUI8RCH8/exIH6axpkJ8Kob+xs0YdY0KUZbCfbYZVM0/iawtncCv1vPKdxHYwCFTBEfAWsBV3NvvIM6CciV118nD64hiYQ9OuONREp3gfdchj3plR8g0A/yzRiM5VqbACdccnuu/ZueszK53R61BSrp3lFb3Gw3AYW11Sgpp3grVAT4nPcBfKQdXz+ZjKwt1JoCVzhZZPxjenufL9qoXMnj8UzW7IgxCTsty2vXFc7sy9/K4Btz4xi2BD7UWv5u952qKpX2lKrtAWNiNHvBV97u40G5HuyF7hXaJbpYEjI/+M3tWoRnMZ1St5p53LWpEbw8dwjHM8HZpGf+OzjNpBuVPOsrVbtqP08dSePspI1X5ydCScQClzAOEwi2W9tVbH1p4ljGCiQRH5xwHeFwDpCQMFv1FzC/+S2QRE/gDCnwiU4BK5lgCyLRMzhDEjbnQDkYXvX4SWxdmDg453zUorM9CMEp0ykgDdeFeUkUxyxZZHoDJ1DCFrIUN3YrFImu9F42DTw6mcX7H63wIhO5KNKNdr6gbsPVyhPSftcj5yQ5QIinWCrcUMeQ1SsQVbaShK9JxUweFQGWzqxeB2WM5cp1RYCBWToDpQFwKLlYEqpNgHU7S2eweu2XsSglDM83QpsAY3JYUtbF/pDghZEsd0Ovl9gpy3XrAD9MOiOgCTDLXOE0JeYyNfVyOvjTbFn68q1cQ5kjoHv5ydH4Lx+nXhKSSH8+z/8AZA0nSNo9EncAAAAASUVORK5CYII=',
                isChoice:0,
                choiceItem:'',
                suggestContent:'',
                buttonArrs:[],
                totalAmts:'',
                Env: WXEnvironment, // 获取设备环境变量
            }
        },
        props: {
            buttonArr: { // 
                type: Array,
                default:[]
            },
            // totalAmt:{
            //     type:String,
            //     default:''
            // }
        },
        created(){
            // //modal.toast({message:66666});
        },
        mounted () {
            // //modal.alert({message:buttonArr})
            // const data = JSON.parse(this.buttonArr)
            this.buttonArrs = this.buttonArr;
            // [{
            //    title:'审批不通过',
            // },{
            //    title:'提交后级',
            // },{
            //    title:'审批通过',
            // }],//单选按钮数组
            this.choiceItem = this.buttonArrs[0]
            this.totalAmts = this.totalAmt
        },
        methods: {

            clickChoice:function(data,index){
                this.choiceItem = data
                this.isChoice = index
            },

            onSubmit(){
                // //modal.toast({message:33333})
                // //modal.alert({message:this.choiceItem});
                this.$emit('onTransmitSubmit',this.choiceItem)                
                // return false;
                // const self = this
                // const choiceItem = self.choiceItem
                // let retreatStat = ''
                // let fileType = ''
                // if(choiceItem.code == '09'){
                //     retreatStat = '96'
                //     fileType = '12'
                // }
                // let submitData = {}
                // submitData.url = 'asmg.spv.flow.investFlowForMobile';
                // submitData.params = {
                //     tradeId:choiceItem.tradeId,
                //     totalAmt:self.totalAmt,
                //     suggestion:self.suggestContent,
                //     submitType:choiceItem.code,
                //     retreatStat:retreatStat,
                //     fileType:fileType
                // };
                // submitData.showLoading = true
                // modal.confirm({
                //     message: choiceItem.content,
                //     okTitle:'确定',
                //     cancelTitle:'取消'
                // }, function (value) {
                //     if(value == '确定'){
                //         weexRequest.getRequestResult(submitData, res => {
                //             if(res.exceptionCode == '200'){
                //                 //modal.alert({
                //                     message: '操作成功',
                //                     okTitle: '确认'
                //                 }, function () {
                //                     navigatorPopEvent()
                //                 })
                //             }else{
                //                 navigatorPopEvent()
                //             }
                //         })
                //     }else{
                //         self.$emit('onCloseDialog',false)
                //     }
                // })
            },

            onBlur:function(){
                if(this.totalAmt > this.totalAmts){
                    modal.toast({
                        message: '修改金额不能大于原始金额',
                        duration: 0.3
                    })
                    this.totalAmt = this.totalAmts
                }
            },

            stopPreventEvent:function(e){
                this.$emit('onCloseDialog',false)                
                e.stopPropagation()
            },

            onCloseDialog:function(){
                // this.$refs.inputtext.blur()
                // this.$refs.textarea.blur()
                this.$emit('onCloseDialog',false)
                // if(this.Env.platform !='iOS'){
                //      weex.requireModule("commonEvent").hidekeyBoard()
                // }
            },
        },
    }
</script>