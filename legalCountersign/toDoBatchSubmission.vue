<template>
    <!-- 格式批量提交用印状态、格式批量未提交申请 -->
    <div class="wrapper" @viewappear='toRefreshBatch'>
        <oaHeader :title ="title" :leftIcon="backImage" :iconLeftWidth= "iconLeftWidth" :iconLeftHeight = "iconLeftHeight" v-on:clickEventLeft="back"></oaHeader>    
        <scroller class="detail" show-scrollbar="false">
            <div class="header">
                <text class="TitleText">申请信息</text>
            </div>
            <!-- 详情list -->
            <div class="DetailCell" v-for="(list,index) in DetailLists" :key="index">               
                <text class="TitleText TitleTextFZ" :style="{'paddingTop': index===0? '48px': '40px'}">{{list.title}}</text>
                <text class="contentText TitleTextFZ " :style="{'paddingTop': index===0? '48px': '40px'}">{{list.content}}</text>
            </div>
            <div class="blank1"></div>
            <div class="header">
                <text class="TitleText">文件信息</text>
            </div>
            <!-- 文件信息 -->
            <div v-for="(item,index) in showFileList" :key="index">
                <div class="certificatesBlock" v-if="item.templateType=='ND' ||item.templateType=='NS'">
                    <div class="blockTitle">
                        <text class="blockText TitleTextFZ">批量/格式文本</text>
                    </div>
                    <div class="blockName">
                        <text class="blockNameText TitleTextFZ">模块名称</text>
                    </div>
                    <div class="blockTitle blockNameContent">
                        <text class="blockText TitleTextFZ">{{item.templateName}}</text>
                    </div>
                    <div class="typeInfo">
                        <div class="typeInfo1">
                            <text class="typeInfoText typeInfoTitle TitleTextFZ">类型</text>
                            <text class="typeInfoText typeInfoContent TitleTextFZ">批量</text>
                        </div>
                        <div></div>
                        <div class="typeInfo1">
                            <text class="typeInfoText typeInfoTitle TitleTextFZ">时间</text>
                            <text class="typeInfoText typeInfoContent TitleTextFZ">{{item.regDate}}</text>
                        </div>
                    </div>
                    <div class="wordItem wordItemTop">
                        <div class="wordItemTitle">
                            <text  class="wordItemText TitleTextFZ">发送印控文件列表</text>          
                        </div>
                        <div class="wordItemTitle wordItemShow" >
                            <image class="wordItemContentIcon" :src="wordIcon" />         
                            <text  class="wordItemContentText TitleTextFZ" @click="filePreview(1,item)"  >{{item.showFileName}}</text>          
                        </div>
                    </div>
                </div>

                <!-- 备案文本 -->
                <div class="certificatesBlock" v-if="item.templateType=='BA'">
                    <div class="blockTitle">
                        <text class="blockText TitleTextFZ">备案文本</text>
                    </div>
                    <div class="blockName">
                        <text class="blockNameText TitleTextFZ">备案文本名称</text>
                    </div>
                    <div class="blockTitle blockNameContent">
                        <text class="blockText blueFont TitleTextFZ" @click="filePreview(1,item)">{{item.showFileName}}</text>
                    </div>
                    <div class="typeInfo">
                        <div class="typeInfo1">
                            <text class="typeInfoText typeInfoTitle TitleTextFZ">类型</text>
                            <text class="typeInfoText typeInfoContent TitleTextFZ">备案</text>
                        </div>
                        <div></div>
                        <div class="typeInfo1">
                            <text class="typeInfoText typeInfoTitle TitleTextFZ">时间</text>
                            <text class="typeInfoText typeInfoContent TitleTextFZ">{{item.regDate}}</text>
                        </div>
                    </div>
                </div>

                <!-- 证照领用 -->
                <div class="certificatesBlock" v-if="item.templateType=='ZZ'">
                    <div class="blockTitle">
                        <text class="blockText TitleTextFZ">证照领用</text>
                    </div>
                    <div class="blockName">
                        <text class="blockNameText TitleTextFZ">证件名称</text>
                    </div>
                    <div class="blockTitle blockNameContent">
                        <text class="blockText TitleTextFZ">{{item.templateName}}</text>
                    </div>
                    <div class="typeInfo">
                        <div class="typeInfo1">
                            <text class="typeInfoText typeInfoTitle TitleTextFZ">时间</text>
                            <text class="typeInfoText typeInfoContent TitleTextFZ">{{item.regDate}}</text>
                        </div>
                        <div></div>
                        <div class="typeInfo1">
                            <text class="typeInfoText typeInfoTitle TitleTextFZ">水印内容</text>
                            <text class="typeInfoText typeInfoContent TitleTextFZ">{{item.waterMask}}</text>
                        </div>
                    </div>
                    <div class="wordItem wordItemTop">
                        <div class="wordItemTitle">
                            <text  class="wordItemText TitleTextFZ">发送印控文件列表</text>          
                        </div>
                        <div class="wordItemTitle wordItemShow" >
                            <!-- <image class="wordItemContentIcon" :src="wordIcon" />          -->
                            <text  class="wordItemContentText TitleTextFZ" @click="filePreview(1,item)">{{item.showFileName}}</text>          
                            <!-- <text  class="wordItemContentText" >{{item.showFileName}}</text>           -->
                        </div>
                    </div>
                </div>
            </div>

            <div class="header">
                <text class="TitleText">发起人意见</text>
            </div>
            <!-- 详情list -->
            <div class="DetailCell" v-for="(list,index) in initiatorOpinionList" :key="index">               
                <text class="TitleText TitleTextFZ" :style="{'paddingTop': index===0? '48px': '40px'}">{{list.title}}</text>
                <div v-if="list.title=='文件名称:'">
                   <text class="contentText blueFont TitleTextFZ" @click="filePreview(2,fileDown)" v-for="(fileDown,inde) in list.content" :key="inde" :style="{'paddingTop': index===0? '48px': '40px'}">{{fileDown.fileName}}</text>
                </div>
                <text v-if="list.title!='文件名称:'" class="contentText TitleTextFZ" :style="{'paddingTop': index===0? '48px': '40px'}">{{list.content}}</text>
            </div>
            <div class="blank1"></div>

            <div class="header">
                <text class="TitleText">流转意见</text>
            </div>
            <div class="blank1"></div>
            <!-- 流转意见list -->
            <div class="opinionCell" v-for="(opinion, indexs) in opinions" :key="indexs">
                <div class="title">
                    <div class="circle">
                        <text class="name">{{opinion.regOperName}}</text>
                    </div>
                    <div class="state">
                        <text class="stateText" :style="{color: 'rgb(153,153,153)'}" v-if="indexs===0">审批意见</text>
                        <!-- <text class="stateText" :style="{color: 'rgb(0,164,234)'}" v-else-if="opinion.descType==='3'">反馈意见</text> -->
                        <text class="stateText" :style="{color: 'rgb(0,164,234)'}" v-else-if="opinion.descFlagName == '同意'">审批通过</text>
                        <text class="stateText" :style="{color: 'rgb(0,164,234)'}" v-else>审批不通过</text>
                    </div>
                </div>
                <div class="content">
                    <div class="line" v-if="indexs!==opinionLength-1"></div>
                    <div class="block">
                        <div class="firstLine">
                            <div class="leftIcon">
                                <image class="icon" :src="startIcon" v-if="indexs===0"></image>
                                <image class="icon" :src="passIcon"  v-else-if="opinion.descFlagName == '同意'"></image>
                                <image class="icon" :src="nopassIcon" v-else></image>
                                <text class="approvalContent">{{opinion.regOperName}}</text>
                            </div>
                            <div class = "Time">
                                <text class="date">{{opinion.regDate}}</text>
                                <text class="time">{{opinion.regTime}}</text>
                            </div>
                        </div>
                        <div class="SecondLine">
                            <text class="result">{{opinion.descFlagName}}</text>
                        </div>
                    </div>
                    
                </div>
            </div>
            <div :style="{height:'48px',backgroundColor:'#fff'}"></div>
            <div class="blank2"></div>
        </scroller>
        <div :style="{height:'48px',backgroundColor:'#fff'}"></div>
         <!-- 当前数据的申请人为当前使用者,撤销申请,提交用印 -->
        <div  class="btnbg">
            <div class="submit3" @click="sealSubmit" v-if="showBottmomBtn">
                <text class="buttonText">提交用印</text>
            </div>
            <div class="submit3" @click="sealSubmitApplication" v-if="!showBottmomBtn">
                <text class="buttonText">提交申请</text>
            </div>
            <div class="submit4" @click="sealCancel" >
                <text class="buttonText buttonTextCancel">撤销申请</text>
            </div>
        </div>
        <!-- 审批框 -->
        <wxc-overlay  v-if="showOpinion"  :show="true"  :canAutoClose="false" :hasAnimation="false"></wxc-overlay>
        <div class="opinionInput" v-if="showOpinion">
            <div class="head">
                <text class="TitleText">审批</text>
                <image class="closeIcon" :src="closeIcon" @click="closeOpinion"></image>
            </div>
            <div class="buttons" v-if="descType!='3'">
                <div class="button left" @click="agree(1)" :style="{backgroundColor:buttonResult? 'rgb(0,164,234)':'rgb(255,255,255)'}">
                    <text class="buttonContent" :style="{color: buttonResult? 'rgb(255,255,255)':'rgb(0,164,234)'}">同  意</text>
                </div>
                <div class="button right" @click="veto(1)" :style="{backgroundColor:buttonResult? 'rgb(255,255,255)':'rgb(0,164,234)'}">
                    <text class="buttonContent" :style="{color:buttonResult? 'rgb(0,164,234)':'rgb(255,255,255)'}">否  决</text>
                </div>
            </div>
            <div class="opinionDialog">
                <textarea class="dialogInput" v-model="output" placeholder-color="rgb(204,204,204)" placeholder="点击输入反馈意见" v-if="descType==='3'"></textarea>
                <textarea class="dialogInput" v-model="output" placeholder-color="rgb(204,204,204)" placeholder="点击输入审批意见" v-else></textarea>
            </div>
            <div class="submit2" @click="chooseButton" >
                <text class="buttonText">审批</text>
            </div>
        </div>
    <!-- </div> -->
    </div>
</template>

<script>
import oaHeader from '../../component/oaHeader'
import { navigatorPushEvent, navigatorPopEvent,navigatorPushFullScreenEvent,navigatorPushEventurlNo,navigatorPushEventFull} from '@/utils/navigator.js'
// import { filter_array } from '../../../utils/tool'
import { WxcOverlay, WxcPopup } from 'weex-ui';
import { weexRequest } from '../../../utils/weexAxios'
const storage = weex.requireModule('storage')
const modal=weex.requireModule('modal')
const sendUpdateFlag = new BroadcastChannel('update')
const picker = weex.requireModule('picker')

export default {
     components:{
        oaHeader,WxcOverlay,WxcPopup
    },
    data(){
        return{
            backImage:'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABUAAAAkCAYAAABmMXGeAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAAAZdEVYdFNvZnR3YXJlAEFkb2JlIEltYWdlUmVhZHlxyWU8AAADKGlUWHRYTUw6Y29tLmFkb2JlLnhtcAAAAAAAPD94cGFja2V0IGJlZ2luPSLvu78iIGlkPSJXNU0wTXBDZWhpSHpyZVN6TlRjemtjOWQiPz4gPHg6eG1wbWV0YSB4bWxuczp4PSJhZG9iZTpuczptZXRhLyIgeDp4bXB0az0iQWRvYmUgWE1QIENvcmUgNS42LWMwNjcgNzkuMTU3NzQ3LCAyMDE1LzAzLzMwLTIzOjQwOjQyICAgICAgICAiPiA8cmRmOlJERiB4bWxuczpyZGY9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkvMDIvMjItcmRmLXN5bnRheC1ucyMiPiA8cmRmOkRlc2NyaXB0aW9uIHJkZjphYm91dD0iIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtbG5zOnhtcE1NPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvbW0vIiB4bWxuczpzdFJlZj0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL3NUeXBlL1Jlc291cmNlUmVmIyIgeG1wOkNyZWF0b3JUb29sPSJBZG9iZSBQaG90b3Nob3AgQ0MgMjAxNSAoTWFjaW50b3NoKSIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDo1QTExREM0MjREMUIxMUU4OEY4OEY2NDBGOTMyMzY0NiIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDo2QzZEMjY1RTREMUYxMUU4OEY4OEY2NDBGOTMyMzY0NiI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOjVBMTFEQzQwNEQxQjExRTg4Rjg4RjY0MEY5MzIzNjQ2IiBzdFJlZjpkb2N1bWVudElEPSJ4bXAuZGlkOjVBMTFEQzQxNEQxQjExRTg4Rjg4RjY0MEY5MzIzNjQ2Ii8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+uGGeXQAAAmhJREFUSEullsuLjWEcx5+ZE8WCXFZnWLvt3akxFAqFUu7EyZIZMwv/gQwTK52IYcFGUeRSLNxjyYYdZSWXDcrE8fn+nvc53nN9b5/69M7vXXzn6Xmf3+85PbVazTVTrVajv1IzGc/iTjzfqzcFmYq38AhOw8GiobPxEa63ynOjSOgcfIJLrPJcwT15QxfiS5xvlecM7seJPKHLUCvss8ozjENoXz1r6EZ8iDOtcu4PHsBRqyKyhO7FmzjFKud+4Ra8bFWMtKGDOI6TrHLuG67FO1Y1kRTag6fwtFWeT7gan1vVhm6hJbyEx63yvMPl+NaqDnQKDV2yzyrPK1yFH63qQkvv0/czeNxGrShwH7fiT6sSaFgpgaFL4oHXcDOmChT1UALn8dDmL7IXHk2eXfjbqpRYKIGLeTzFuaojTuBRbJ2NCZTK5fI6nvdwur1x7i8exnNW5UArvY762kJdsg0vWpWTdkdqInrmRqG6ArRCob7W+dQIy01vpVLRGVyDX+3N/07SOMtF/fBzAjR4H2B8To5hfU6mpaGjosOv4AX2wqMr4hCm3ut2bTqLh9p0qb3w3MXtmL1NBXv8hccA6uwGNqAmvv5hIu2OlNCKNNWvWuXRyh+jtqgrnUKF+l2jTx8roI/5AuN73kK3UKEN11UyYpUnTLL4njeQFBrQlXIQNReE9la/TLTXLaQNFWoI7XNz9+22KkaWUKGjpqn23Sp/u+pjHrMqImuoeIa6TXWrBvST5yTq9s0VKt7gCnxvlUcfUyOzlDdUfMCV+Noqj34CjRcJFZ+xHzUvAjuKhoofuAkvICfDjf0Djb6D75MHfhwAAAAASUVORK5CYII=',
            title:'详情',
            iconLeftHeight:'40px',
            iconLeftWidth:'25px',
            pdfImg:'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGAAAABgCAYAAADimHc4AAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMDY3IDc5LjE1Nzc0NywgMjAxNS8wMy8zMC0yMzo0MDo0MiAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTUgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOjI3NzVGRDlENUZGQjExRThBM0EzRUZFRTM5MDE1NEYwIiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOjI3NzVGRDlFNUZGQjExRThBM0EzRUZFRTM5MDE1NEYwIj4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6Mjc3NUZEOUI1RkZCMTFFOEEzQTNFRkVFMzkwMTU0RjAiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6Mjc3NUZEOUM1RkZCMTFFOEEzQTNFRkVFMzkwMTU0RjAiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz6JmHcUAAAIr0lEQVR42uxd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',
            wordImg:'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGAAAABgCAYAAADimHc4AAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMDY3IDc5LjE1Nzc0NywgMjAxNS8wMy8zMC0yMzo0MDo0MiAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTUgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOjJDOEYxMzJENUZGQjExRTg4QkI1RjA1MTdDQzVCM0M5IiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOjJDOEYxMzJFNUZGQjExRTg4QkI1RjA1MTdDQzVCM0M5Ij4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6MkM4RjEzMkI1RkZCMTFFODhCQjVGMDUxN0NDNUIzQzkiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6MkM4RjEzMkM1RkZCMTFFODhCQjVGMDUxN0NDNUIzQzkiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz7mxLCVAAAHeUlEQVR42uyc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',
            excelImg:'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGAAAABgCAYAAADimHc4AAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMDY3IDc5LjE1Nzc0NywgMjAxNS8wMy8zMC0yMzo0MDo0MiAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTUgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOjMzRkJCNUFENUZGQjExRTg5RDZBOEU5QjU3RTA3QUFBIiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOjMzRkJCNUFFNUZGQjExRTg5RDZBOEU5QjU3RTA3QUFBIj4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6MzNGQkI1QUI1RkZCMTFFODlENkE4RTlCNTdFMDdBQUEiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6MzNGQkI1QUM1RkZCMTFFODlENkE4RTlCNTdFMDdBQUEiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz4EJV2KAAAHMUlEQVR42uyc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',
            txtImg:'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGAAAABgCAYAAADimHc4AAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMDY3IDc5LjE1Nzc0NywgMjAxNS8wMy8zMC0yMzo0MDo0MiAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTUgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOjQxMUNFQTZENUZGQjExRTg5RDgwREY1QUFDQ0VERjk5IiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOjQxMUNFQTZFNUZGQjExRTg5RDgwREY1QUFDQ0VERjk5Ij4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6NDExQ0VBNkI1RkZCMTFFODlEODBERjVBQUNDRURGOTkiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6NDExQ0VBNkM1RkZCMTFFODlEODBERjVBQUNDRURGOTkiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz4pgP/8AAAEaklEQVR42uydUWhTVxjHv5vcNLeNxcbayuhmWTesOFuxInMbgzkZwoZzDCqOCRtsPumL+CBjm4hsQ9zDQBRxTITp2jAFH3ypVEenVpgbHbYPs5MFEdtic9PGNGma3N6c3e9k6VKpmILNScz/B+XmHm5y4fxy/uc7p7dUE0K4aR64O0L76emicpnhOjr8fu2Qipu7qMwRgtpGEukrzRfG/BCgjsbbUfvahq6IDwJUjQSi5hthq2fb1WgFBKiTsPbc3eTFA/1xNwSok/D6wYHJ8z33LQ0C1El4e9PlyI8QoJC0oA+ckvs4BKiVsNORcAgC1ErYq3eEPoMAdWi2oAOOhN0QoA63I+FbT0foIwhQh2da0LGKzlA7BKjDsNL0fUWnuQUC1FFtpcVJb8DcBAHqWJKyxenKgPkKBKijfsoWgaqA2QYB6mhwJHTW/Gy2QoAiBNELUUucXnLWXAUB6iS8FEmJU0vPhVdAgDoJ68aS6RPzfZ9e9vsMGvVqRL2q7q9cwMfLPP4dLxoyPwfC0yMLVjcaLqPB5/LHLJF470asr1i+AMoFrPHr/o2NhlzUbGxc+PsNx+ygcygaAZgDIKC8UR5Boal0gmNhaMIeiaXSU4+bB958zrtqdZ1n1urzSF/szOPu01KrP8NHngMgIIdv7iRHnJ8z+V5/SXakZ1bbnluJYB5vDRbjCEAEQQAEAAiAAAABEAAgAAIABEAAgAAIABAAAQACIABAAAQACIAAAAEQACAAAgAEQIBSGhdnnvHM5buVlU0QUCAMXTOephFQcn+idLw/frnOcF3PbTs/lBqGgALBT1NjDgAQAAEAAiAAQAAEAAgoZbT5/gcN7afQNLrt0YgP63SMAEQQgAAIABAAAQACIABAQBGj9FeSzqoxr+v6Ry1a0x2hm2/VUGu9Z+b8Yf55x09NNTqdHZykbX/EZ67Pc4VffgKu30tS190kjaWEPN+y3Eubmww6ORCnP8P/73iEk2l5bL/ygPq21spO3dVQQceGUjPX/NBaJTs/GJmmLwYmZdvguEWXnM8Pxmx5vrZWp09afHQxOEUXnPayHwGv/Rqddc4dxHDn53Zulr+Tgvb1RunoG4tp//pF1G2Oy7YVXo22r6yS13z+24RsY3gU5LLrv+O9uD3n52MOyAPuOI6Yep+bvmrJdPqpDdXk82hy5ATM0torLMlJmCNm1PkWtzdXyeh59VmvjJ5P+ydRBRUCjpiDv8fka870uCVo59UoytBC0m1achQw953jL1EbAgoJ5z/PA/zt5+qHowgCCgSXoJz/PALe7RqXEjiKti/VIWCh4ZKTS1CG5wGOnsCtzOT79cvVELDQHGlbJKOHS9FsLc/VD6+OSzGKSkrAl89nVsocOdnVbpY9zgIsG0UcURDwhOF837c+EzEnbsZnVrtZcqOII4qjqhQoqlmr405SbkNwifkwo6nMNkR2NTwXhwcTs/aQ5ipdd/c8oL8miqdkxXNBTxg8F4QqCEAABAAIKBH+FWAAZuKwvEFTb5EAAAAASUVORK5CYII=',
            jpgImg:'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGAAAABgCAYAAADimHc4AAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMDY3IDc5LjE1Nzc0NywgMjAxNS8wMy8zMC0yMzo0MDo0MiAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTUgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOjNBRTM0OUFENUZGQjExRThBMTc0QjEyNERBNUM4MUY4IiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOjNBRTM0OUFFNUZGQjExRThBMTc0QjEyNERBNUM4MUY4Ij4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6M0FFMzQ5QUI1RkZCMTFFOEExNzRCMTI0REE1QzgxRjgiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6M0FFMzQ5QUM1RkZCMTFFOEExNzRCMTI0REE1QzgxRjgiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz6Gwx16AAAHpklEQVR42uxd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',
            lineImg:'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAQAAACICAYAAADAg0rdAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMDY3IDc5LjE1Nzc0NywgMjAxNS8wMy8zMC0yMzo0MDo0MiAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTUgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOjg0RTIwQTQxQUNGNDExRThBMjM3RDJCNzQyM0Q1Njc0IiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOjg0RTIwQTQyQUNGNDExRThBMjM3RDJCNzQyM0Q1Njc0Ij4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6ODRFMjBBM0ZBQ0Y0MTFFOEEyMzdEMkI3NDIzRDU2NzQiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6ODRFMjBBNDBBQ0Y0MTFFOEEyMzdEMkI3NDIzRDU2NzQiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz44S1j8AAAB+klEQVR42rSVyy4DYRTHZ9op0R0h4QV6Ebc3oFQ9Adpa8AyIHWl4j+qirdsTCG15Ayypii0bLAibOmfyP3J8TZPJDJOcfjP/OZeeM798n22Vn2zLsnJkvFYd+smTlS1cIcu42KOqnqt2u93+5dER0pnDrjyP0HqAsllOmiWbhoMr1Mje4FFzq1BYD79urwx9eSxLIUk29566TdN6CocFrhJToTEWSmRxlC35bC5K6x6etznpGtk6hHsWbrkNCHfS3ASau/HeXIRNmuP4CzjMcJU5sn4IaRYOyRYhHPhsToBxc5jA2GaILcCEMNNKAGAEvLwAMwMhJ8C8oMq5zLQXM/30PtMEW1dg4io0wcI+36BsMQAwu3je4aSrZBsQWiw0FTBNaW4SzV37BGZcAZPiKtztgAbmiGwJwmEAYLKSwwE9FQ2MDgs7eBuBUP4DYHImMC5BdbJXCD87TBQzffc+0zibzHSW1jM4ZLjKqApNslDkGwh+gekzgeEdZhPCAwstBcy9NDeF5q68N8fVHGlujNZLvcNkFDDzJjBHAYBZlhzy5eRThkI4hboC43eHGTZ3GOY0pYWGAqYuM+3DTD/+DZgSdhgrwJHE/7KA54IAswXhkYUHBUwrwA7TAUwDDrMCzKAG5lgdSccBgJEcJyYwYRMYR44kAab6LcAAe+wCSY5/JOIAAAAASUVORK5CYII=',
            startIcon:'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAYAAABzenr0AAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMDY3IDc5LjE1Nzc0NywgMjAxNS8wMy8zMC0yMzo0MDo0MiAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTUgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOjFERTM5RDkxQUNGNDExRTg5Qjc1ODNFN0IyNzc1NEQ3IiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOjFERTM5RDkyQUNGNDExRTg5Qjc1ODNFN0IyNzc1NEQ3Ij4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6MURFMzlEOEZBQ0Y0MTFFODlCNzU4M0U3QjI3NzU0RDciIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6MURFMzlEOTBBQ0Y0MTFFODlCNzU4M0U3QjI3NzU0RDciLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz47IUBpAAADUElEQVR42rSXTUhUURTH37xMoiZTLDTaODZmYPYBtcimTPsgpigoaFFGi4KBVlGLskWZUFEgtYsW06IPjKJNObWIhiCtFIpyIVYzDVmgk0FlqTVg9j/wf3EZfO/dl9OBH+/Ne++ec+eec8851zd8qtbQFD/YBOrAEhDkM5EfIAlegkfgPp+5Sp7GNwFwBOxWDGbLDFACasABGr8OzoKUk3LT4d00cBr0ggiNPAHHuArzOCE/7+v4roPfRjj2DHVNKD4bF5SD22Ap+A2uUVGvprsWgkbQwD8prtkB3umswCL+CzH+BoTAXg/GDX4rY1aBt9TVQd2OE5DAioNS8BCsAE+Nf5dnYDl1lVJ30G4C4qebYA5oB2EwZExehqgrTt231JhQJ3ACLAN9YDvIGLmTDHVa7mjKnsB8cJgBtxMMGrmXb2APbRyizb8TkH0+FVwBnR6UlhBd6aQNsXXU2oayjwfAdFDJZdKVS7xGPIypAK/BCJgrK7CZiaPdo/FqsI9UexgnNh7TZlgmUM8XbR592gKmkBaPY2O81uexsFh71k0KGcXrwAbludw3c6tJ1vuqkR9EFksMSMTPZqJI2wxoZloNaP7DFNP3cYfglbj7LC4o4EOnWV8EYx6WeIxj7OQLrwWmpsJ+xkqfxrfvWRn7dRSbSrotdPn2A33vlKQ+gfXgo4uuIitNm0qJrNCYcIK+s5MBfqOTC0SSMoFX/LFS0x1lLt2TjtTw2m2yVBpMSG5SDGbyfhzcBXd4b/BdsYaeMK9xmcA9MAzWZNdqm3/4C0TZXGwF23gf5bsyFx1B2pJUHJMJfAc3pC6wjXKSNNu1/aBHed7DZ+UOucSSRtpqFdtWTxikEpP+6TL+j1gdlpTkKqkLphLd55nXW9m55FokPq7SxgWr8KmJSLqUbi6jtE35OTSezy67kjaaJmrJRtk6S6KpZYT7c2Bc6swDFqxB2hix64oTPH5JIG0Ez+m3yfi8i1Gfpu6E27ngBc8CslQLWDova2ZKNdNFOTZAXSHq1joZWW36SXCQPhxnJxNj95RUqloRm8wQE9pqbrUMA0467p9ejmbZiUOa1l3sG3VklIfTc25tns/j8XwLWMuuSJZ2ltJyp5TjeZvu8fyPAAMAU3DF8XNOoCIAAAAASUVORK5CYII=',
            passIcon:'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAwCAYAAABXAvmHAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMDY3IDc5LjE1Nzc0NywgMjAxNS8wMy8zMC0yMzo0MDo0MiAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTUgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOjJDNUEwMjExQUNGNDExRTg5MUQ1OTY1RkU4QjhENTk4IiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOjJDNUEwMjEyQUNGNDExRTg5MUQ1OTY1RkU4QjhENTk4Ij4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6MkM1QTAyMEZBQ0Y0MTFFODkxRDU5NjVGRThCOEQ1OTgiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6MkM1QTAyMTBBQ0Y0MTFFODkxRDU5NjVGRThCOEQ1OTgiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz5QIo/kAAAFkklEQVR42sRaWWxVVRS9fZZBWymFVq0gH1oGE0IBa0AMkWJSo6IfCoppGwccEghWFBVNFI2xiMHpSyRKaEXQOAt8aIyiCUiDEqolUkFNQFtFKGhraS0Oa+s6uri+4b737n3sZCXvnHt7ztpn2NNtnrf2gBeClAIzgKnAucDZwHCgkM+7gUPAN8CXwDZgM/BTthPnZaFAMXA9cB1w/t9jpSd/AtuB9UAT0JkrBc4A7gZulRXOVnqA1UAD0BGVAvlAPfBQHOK/Ac3AB8DnwFck8iufFwBlwBhgAjATmAIM9I1jR+1h4GngWJgKjOZWn+frbwGeB9ZlcASG8fjdAlT4nu0A5gJ7wlDgCmAtMET6dgH3Axt4lrO7h//M8SgwXvp/AWo5R0KJpRh8HvCmkO8F7gUmAu+EQN5dZhtrEsfuZf8Qzj0v0x24DXhWrMvXwGxgpxet2OK8BpwjCs4HVqajwJXAG8BJbH8GXA786OVGTgc2yZ37HbiKO5VSgXLgU6BIyF8M/OzlVmz+94FKuROm0N5kd8BM5ctC3jznpSeAvMc5LyMHdydeAQYkU+AO2Taz7deE4e6zEJt7DrmYTKYvinuEyuiAnJNaDDyRA5ID6bT+SPLOncKlmw6xw78D9wn5FnrDqKUOOAK08eImkmfE+hXSBx23AxY57gdOFue1MWLypdzxoWzX0KMnklni1I4CZ1mE63agVsi30IRFLQ1CvofhdTLZRG4eudbpEaqRF58LycOmclY3Sns50B7AY6+Sdo07QqV0UOZx+xkud0ZI3ub5CJjO9j4mQT0BA8AOXnxTaKTtQJWEC59ETN5jODJd2osDkvfIbZssRJUpcIG8sDli8qcAK6T9MfBqmmN8KL8rY7SpTqIO1Gy1R0l8U5/BGC3ye4xfgbYIyY9iuOxkVYYL1uZX4DTpiDLaXM4j5NF5Lc1wHOVYGvNlWl0RkbdLe620l2YRYynHIjOjavMHSeAUlsQYnk+SdHRi0KQ9QezU5wLOGIMjJ6cGHGQsa0LDA7x7k5B3gdmxLBZEOXbl80wVSiZ0KMUAJUxyCphcVAPfJklKGqT9FvBeCNmakyMxesJ/b3WAAUpI3mVvW1jriScPMGjzuO13hXAkleM+U6BVOioCDLAbeETaZXSAF/reGwfcLu2nJLvKRnSxdpkCW6WjKuAgDwKLJOgr5tGYJe+skPTve9Z9wpCZ8rvZrJD5gR8YW/TxjAXNgS2kfUGI9rOO0+nLJ24AGkMgbyb/AK2lyYgYO3aIGZ2dxoAvstzhgrEBJLpG3rHgqymk1Z8j5I1zu8sH1stL89McdCMt0WGJEkskhl8YYn6h3NZpQtPINM1l/tVpDmyW6CLv/6XxJjqxMKSa3FxK2agKHPRt++OsEaUjX9AS7ZVC1JKQyOeTk5M15HxcWWUEIz1n481mP5nBZENZT9rqM9HZyCLh0sNI4Du/Ah5XbBl/9zLZiTpHCJI/W6Y4WMo/j8UrbDkrsl0cmh2HaSewOlfKnSxnu5X3oF8jRZV+hr1dEipskGOVSyng3OVydOYq+XgKuIynjimfyRR62WE5JG+e/V3O7dLPGobiXioFTN6m/XYyjVtZkQPyE3jmNbZayEjWC6qAiX2dWSA7MZZetT4DExvUVNrYzZzLrfwCcklQZEr9ke9q2t1CX/ViCbc5DLmE1k8Tn27GUK8nr5IF+8w6jvWb8b5+S2xW8lm6H0GKGNvYt7hK37NWPtuduswX/EP3INrge7z/CsGe+IwtzAt2cuKDrD4451bChTC7PoNnfLBvnKP0uMsk7w1NAScjqcTNcRTJVPp4TC1n2J/OH2bzzx7mZGqJyRmOYSGxfUR/iWF92pIX0r/bnMlMaSotyGgmH8V8fpjB3R76GbNm9n8V7dlO/JcAAwD1UlQSx0fw2AAAAABJRU5ErkJggg==',
            nopassIcon:'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAYAAABzenr0AAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMDY3IDc5LjE1Nzc0NywgMjAxNS8wMy8zMC0yMzo0MDo0MiAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTUgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOkU2MjI2QTlEQkIyNDExRTg5QTNBQTk0MEZDQUI0NjdCIiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOkU2MjI2QTlFQkIyNDExRTg5QTNBQTk0MEZDQUI0NjdCIj4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6RTYyMjZBOUJCQjI0MTFFODlBM0FBOTQwRkNBQjQ2N0IiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6RTYyMjZBOUNCQjI0MTFFODlBM0FBOTQwRkNBQjQ2N0IiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz5LjqRKAAADSUlEQVR42rSXS2gTURSGJ2NblNioC6VgMalpfWzUhVRKUZKgIu3OByIidGUq6EKRaruJLqy6qeBCdFcUFam6EevG2ojEFy5sNz5qU2tdCC60qaIIVv8T/gvDOHdy8/DARx5z7/nPzD33zLmBXPKwZWjzwTaQAOvAchDitRkwDl6CYXAPfDNxWmUwpgEcA3sZhJfNA0tACzhA8avgLJjwc277XJsLesFrkARB8Bj0gDhYyv+C/B7ntQz/S3LuafrytIBmCeTx3uKjngVXwBk6NLGVoBvs403K0uwAWZMnsIZ3IeJvQCvoKELc4rwOzh2jrwx9+wbQCO6DOjAEmsFTq3STuevpq46+G3UBSCINgMXgEWgDOat8y9HXA/oeoNY/AaT4qCa5Xr+sypn42g7eUiPlDiAKjjDhdoPPVuVtmkk5S62oM4DjoBpcBs88Jke5p+sNhMIcG/G49pwa1dTMB1AL9oA/4JTGqezpLvAQLPMRF9E0x+7XjOmllmjW2kyQIBPvnWbSefCe9WFYE0SE1yIsyxc0vsaoJZptNmu72F2fO/sIYj5BqDtX4nHO0ZnSSkgAa/njSYG1nWQQE64glHiYlU7EpwzqQ77oVals5BaxDIKIU1wFMcchHjMQV5UyX/hsxyv1q+F2cj+JYsXFvvAzZJe4pwMeJb0kX7aj3C40nBPmo2/gnWddeWBii1SZtpm1YisMxdMO8RhzotgglNa4BDDKHy2G4hHXmn8oIQilNWrzVSnW7jOhXiOuzCsIv7KttIYkgEHwHWx0v6sddshH3BlEzBHEQY2vRmqJ5qDNjvY6M7tHM+kSe7tNBbbaFMfI2IuaMd3UEs0Z1RNKMXrFotLCt9b/sGZW3N9gtUpCizuhj9vyBjuXStsCNreicU7tPmfxOMnuVdbvJqipoLj4us3tNwJOeLVkP8AudkOyjnfYK5RrIfpK0PdOanl2xdIPbAGfwFbmwoYyxGXuC/oSn5vdPYdX/R5hPy/LsYqnoX7DSumsdP2c20RfrY6iV/BkpI5mKTaQNWyjMmwmMuxsph0J1kSRdn4G2A33Mb9+FnM0cx/TugocTt0mh9NrbE6z5Z6OxUEnOMr+MVbgeJ5mdTU6nv8VYABIgNcUhyZY7AAAAABJRU5ErkJggg==',
            approvalIcon:'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAYAAABzenr0AAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMDY3IDc5LjE1Nzc0NywgMjAxNS8wMy8zMC0yMzo0MDo0MiAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTUgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOjNCRjRFOTYxQUNGNDExRThCNDRGQ0NGQTEwNDNFQTcwIiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOjNCRjRFOTYyQUNGNDExRThCNDRGQ0NGQTEwNDNFQTcwIj4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6M0JGNEU5NUZBQ0Y0MTFFOEI0NEZDQ0ZBMTA0M0VBNzAiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6M0JGNEU5NjBBQ0Y0MTFFOEI0NEZDQ0ZBMTA0M0VBNzAiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz6j5zo5AAACzklEQVR42sSXOWgVURSG7xvjwyU+d8RKNDHRJlER0YiFSSMJgoWpREhlaaGQaERjGkELwVKxUNwQl0rjUkQJPINi8UDBJXlq54aiiah54PIf+AaGyYOZt+bAx53h3jn/nbudcxPu4icX02pFu9gq1ogVIkXdmHgjMuKBGBA/4jhNxOiACXWLXXQijpn4ZXGcjhXVgRmiT+wTSfFPpMVtyhHxjbbzxEqxWXRQJkROnBT94nchHbC/vsFQ/xUXxDHxOuYINIhesVt4TM1OkQ039PJ8vE48QvylaBFdBYg72nbx7Qi+bNTWRnWgXtwVS8R9sUE8dsWbfbseX+bzHhp5OzBTXBOLxZDYLsZd6TaGr0F8X0drUgf6GKp3zFfOlc/M1w6mplkcDS/COvFCTBObxBNXGbMpHRZ/xGpblP4IHBDTxfkKijt8n0ProD8Cc1S+F7PYPqOuslbPVPwUSz2O19ksvEqLOzSG0Gy3DrRRMeCqZ75Wm3WgiZdhSjt2T4kv4ivPyZCDqDZR9b5WUw07wAVOuhNib6CxPf9iobqYbaLqfa06W4QT9M6CzwS9XhD6Y/uLhYH3qDZR9Um0cp6bYvM4Kv2Q6oh8YTsbeo9qE1U/3z+mawiRizgDPpJ8JAilvrPDIWdRbaLqG/0taWvgtB72iB4WTzWsm2zpjEeUcmQy1TJfa3AqjmJL3V4Fj2KL+VeYs0NV+PtetExzPF84bikxCyoqHGfJXu39KplLuW0uu8FDKxvOiPrJXpeRESfLKG6+brLGMmhNSsnsrO4Un8UWIlaqDOIpfLXiuxOtvFnxKOH5A6VlMBtLELdvnwZ8toZ3Wb5Y8IybTYYTK00ataoA4Ua+SbPtMvh8XujV7IjYH7ia2Qq+RWkh9XtggTWQ0Hawk0q6mgVtOcd0oZfTSxy3b0u9HQev59u4njeTXNYGBLMM9UNxJ+71/L8AAwAMkstov4logQAAAABJRU5ErkJggg==',
            closeIcon:'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAYAAACqaXHeAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMDY3IDc5LjE1Nzc0NywgMjAxNS8wMy8zMC0yMzo0MDo0MiAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTUgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOkJDRTk4MTQwRTNDODExRTg5QjM3OEI5RjBFRDlFMjcyIiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOkJDRTk4MTQxRTNDODExRTg5QjM3OEI5RjBFRDlFMjcyIj4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6QkNFOTgxM0VFM0M4MTFFODlCMzc4QjlGMEVEOUUyNzIiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6QkNFOTgxM0ZFM0M4MTFFODlCMzc4QjlGMEVEOUUyNzIiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz4wCbZaAAACHElEQVR42uyZwU7CQBRF7RQX6sqFce/Cz/BXNKG4lgUaiMQIRgiasHRB8Wv8CROX7liZuKV4STAx2JaWzntT5U4y6VDKm7kn05l3B282m/lbG1w8AiAAAiAAAiAAAiAAAiAAAiAAAiAAAshdxuPxyXQ6DdE8NMb0giDoSw44DMN2FEUNNN993z+vVqsvReKZogOC+GdcjlD3MLC70WjUlRKP2A/o4wbNXdRj9P1UNKaxMK79nx8wo5oSEObiEbu+dPvAOQBM++HyPdsQEOsxRvy870EpFkEM8B5xLn8F97xerVZrWxB/ERO7i9i3pdkFJCCkiO8gZqd026BNCBriRfIAGxC0xIslQkUgaIoXzQTXgaAtXjwVzgPBhXgVL5AFgivxamYoDQIuO67Eq7rBJAixg1ISr26Hs0DQFO/kPCANgrZ4W24wb9le87t/8QrEWVrrBqqsi+BK8S4gaG2DseLn7/xiG2y4gqCRCCWK/17w8EzPFQTpVHil+B/POoEgaYYyi3cJQcoO5xafAUIfv70uPQAbxkYTgu0jMWuuTguCzUNR65Y2xUVag2DrWFzMz0tDKAwgDMNWFEUdSWOTBMEY0wiCYOjUDC3+qBR1dYjVQsxBTN9XZXCDEw1LmwBh4hyA7/tnuLyhfmBK1iX9/BwC+mii+Yn6WqlUTv/cgUjZCgEQAAEQAAEQAAEQAAEQAAEQAAEQwAaWLwEGAB4fUlf6TPQgAAAAAElFTkSuQmCC',
            wordIcon:'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAYAAABzenr0AAADDElEQVRYR+2XXUiTYRTH/88+3eY+dF8utlLDOTSJisLUpHIlhKIQqF3UVRREGRUGQd1UCt5FURDZXZERBGqfVhZhEJhUgz5M0Gxb6pyUc27uyzee6UxrupkbBfnevPD+z3nO7znn4XnPIZh6FDc+LmMIuUQI2Q5AEPq+0PehzKQRi9Off6VA8y4aXxIykjd2PSaEFEXjNJ/N8Ww5SnWJ3uv9bsNZg6w30nozAbyEEG4kh0g6BajJTkb3qNf9YNCpr86QW+bzmQZQ3PzERFo8Gj0EQG27Hb6xAZcnrVAjHprLN64ANGjr1zFz8QtRGipIIBxE3AFo0CcDrh6jRrTyrwHQwO2DrvebUkTZv0LEPAP7MmSoXaMMW/K7VufjEq1420wx5gBaARuvS9PnPK/Xekeu7k6X7Q0ZxByALlypTUTdejUkXFZYkGaz81zZcvERKsYFgC4c8ASwSsqDNAwEASZadqSx4woQ6c6wV+qDm49bBpYAljKwlIH/IwMsAmRJ+ehyeOCbmL1nAZtAL+Gh2+GFK/B7rxPVRXR7sxYvh9yggZQJbFz48A2fx3zBSHvSpShUCzHg9iNPJUBNpw2dw+NBrVSbiKNZyWi3uYNanclOe4JZhFEBNG3RouzpZEunTmDjcq4G5c8syFcKUJEqweGOwaAm5rJwZ6sOJW1mKPls1K9TYddzK/wMwGMRaIUc9DgnwUPPggGo4/kNatSa7DhoSEazeRQdUzum2skcBe5ZnchVCmB1+dBkds57Dv8IoGGjBqfeDGG/XoYWi3M65TQSbUJu9TmQpxTGB6AoRYgTOQoYH33BChEX1YYkHOu0BXe5OomPhjwN8u/3QSfioH6tClVTJaC/Y2pv+u5ZeAlajTq4AwzYhMA27sfpt/bpQ1igEqAqVQKtkAvPBBMsTSiIUSNEuU4MjYADDovgjMmOVzPKRUmiKkGky2Qx+r8HIG/sisloFk1WGIbxDVdl8qhtzIfTqADAtA1XZgYH4Z8tGR3PWeQiASlezHgeAcDNgHlIOPwD9p2p/dT2B0wuWTCXIfZgAAAAAElFTkSuQmCC',
            operId:'',
            DetailLists:[{title:'送往单位:',content:"非格式法律文本"},
                         {title:'申请用途:',content:"借款"},
                         {title:'用印内容:',content:"0"}],
            lowafiles:[],
            backfiles:[],
            opinions:[],
            output:'同意',
            buttonResult:true,
            applyNo:'',
            descFlag:'1',
            index:-1,
            value:'流转',
            chosenButton:'',
            serialNo:'',
            showDialog:false,
            showOpinion:false,
            descType:'0',
            showNode:false,
            indexDetailObj:{},//当前详情对象
            operNo:'',//当前操作人
            indexZZDetailObj:{},//证照类型对象
            indexSDDetailObj:{},//格式类型对象
            indexBADetailObj:{},//备案类型对象
            indexNDDetailObj:{},//批量类型对象
            initiatorOpinionList:[{title:'意见内容:',content:"非格式法律文本"},
                         {title:'时间:',content:"借款"},
                         {title:'文件名称:',content:"0"},
                         {title:'文件上传人:',content:"0"}],//发起人人意见
            buttonText:'',//按钮显示
            reqDetailObj:{},//applyDetail接口返回对象
            userInfoObj:{},//当前用户信息对象
            opinions:[],
            showBottmomBtn:false,//true为显示提交用印,false显示提交申请
        }
    },
    created(){
        // modal.alert({message:weex.config.bundleUr})        
         storage.getItem('setPersonalMessage',  (event)=> {
            this.userInfoObj=JSON.parse(event.data);
            this.operNo =this.userInfoObj.operNo?this.userInfoObj.operNo:'000844';
            this.operId = this.userInfoObj.operNo?this.userInfoObj.operNo:'000844';
            if(weex.config.bundleUrl.indexOf('?')!=-1){
                let path=weex.config.bundleUrl.split('?')[1];
                this.applyNo=this.getQueryString('applyNo');
                // this.applyNo=this.applyNo;
                this.getOpinionList(this.applyNo);
                this.getAllFileInfoByApply(this.applyNo);
                this.getCirculationOpinions(this.applyNo);
                this.getDetailByApply(this.applyNo);
            }else{
                storage.getItem('toDoDetailObj',event=>{
                this.indexDetailObj=JSON.parse(event.data)
                this.applyNo=this.indexDetailObj.applyNo;
                this.getOpinionList(this.indexDetailObj.applyNo);
                this.getAllFileInfoByApply(this.indexDetailObj.applyNo);
                this.getCirculationOpinions(this.indexDetailObj.applyNo);
                this.getDetailByApply(this.indexDetailObj.applyNo);
            }) 
            }
        });  
    },
    mounted(){
        // storage.getItem('setPersonalMessage',  (event)=> {
        //     this.userInfoObj=JSON.parse(event.data);
        //     this.operNo =this.userInfoObj.operNo?this.userInfoObj.operNo:'000844';
        // });
        // storage.getItem('toDoDetailObj',event=>{
        //     this.indexDetailObj=JSON.parse(event.data)
        //     this.applyNo=this.indexDetailObj.applyNo;
        //     this.getOpinionList(this.indexDetailObj.applyNo);
        //     this.getAllFileInfoByApply(this.indexDetailObj.applyNo);
        //     this.getCirculationOpinions(this.indexDetailObj.applyNo);
        //     this.getDetailByApply(this.indexDetailObj.applyNo);
        // }) 
       
    },
    methods:{
        getQueryString(name){
            let url=decodeURI(weex.config.bundleUrl);
            let query=url.substring(url.indexOf('?')+1);
            let vars=query.split('|');
            for (var i=0;i<vars.length;i++){
                var pair=vars[i].split('=');
                if(pair[0]==name){
                    return pair[1]
                }
            }
            return
        },
        toRefreshBatch(){
            storage.getItem('routeObj', event=>{
            var obj=JSON.parse(event.data);
                if(obj.route==1){
                    this.goList();
                }  
            });
        },
        goList(){
            var obj={route:0}
            storage.setItem('routeObj', JSON.stringify(obj), event=>{
                navigatorPopEvent();   
            });
        },
        sealSubmit(){ //提交用印
            var str={type:'0'}; 
            storage.setItem('sealTypeObj', JSON.stringify(str), (event)=>{//sealTypeObj为1表示批量用印 2表示会签用印
            });
            modal.confirm({
                message:'确认提交用印吗',
                okTitle:'确认',
                cancelTitle:'取消'
            },result=>{
                if(result==='确认'){
                    var getData={};
                    getData.url = 'mobile.applyPreInfoInsertContract';
                    getData.params = {
                        applyNo:this.applyNo,
                        regOper: this.operNo,
                            };
                    weexRequest.getRequestResult(getData,(res)=>{;
                        var con=res
                        if(res.successed){
                            if(con.exceptionMsg==null||con.exceptionMsg===undefined){
                                var obj={serialNo:res.returnObject}
                                    storage.setItem('serialNoObj', JSON.stringify(obj), event=>{
                                         navigatorPushEventFull('toDoSealApprovalSubmit'); 
                                    });


                            }else{
                                modal.toast({message:con.exceptionMsg+''})                                        
                            }
                            
                            // setTimeout(()=>{
                            // },1000)
                        }else{
                            modal.toast({message:con.exceptionMsg+''})                                        
                        }
                    })
                }
            })
            
        },
        sealSubmitApplication(){ //提交申请
            modal.confirm({
                message:'确认提交申请吗',
                okTitle:'确认',
                cancelTitle:'取消'
            },result=>{
                if(result==='确认'){
                    var getData={};
                    getData.url = 'mobile.submitApplyApproval';
                    getData.params = {
                        applyNo:this.applyNo,
                        regOper: this.operNo,
                            };
                    
                    weexRequest.getRequestResult(getData,(res)=>{
                        var con=res;
                        if(res.successed){
                            if(con.exceptionMsg==null||con.exceptionMsg===undefined){
                                modal.toast({message: '提交申请成功'});
                                navigatorPushEventFull('toDoList');

                            }else{
                                modal.toast({message:con.exceptionMsg+''})                                        
                            }
                            // setTimeout(()=>{
                                
                            // },1000)
                        }else{
                             modal.toast({message:con.exceptionMsg+''})                                        
                        }
                    })
                }
            })
            
        },
        sealCancel(){
            modal.confirm({
                message:'确认取消申请吗',
                okTitle:'确认',
                cancelTitle:'取消'
            },result=>{
                if(result==='确认'){
                    var getData={};
                    getData.url = 'mobile.repealApply';
                    getData.params = {
                        applyNo:this.applyNo,
                            };
                    weexRequest.getRequestResult(getData,(res)=>{
                        var con=res;
                        if(res.successed){
                            if(con.exceptionMsg==null||con.exceptionMsg===undefined){
                                modal.toast({message: '撤销申请成功'});
                                navigatorPushEventFull('toDoList');

                            }else{
                                modal.toast({message:con.exceptionMsg+''})                                        
                            }
                            
                            
                        }else{
                            modal.toast({message:con.exceptionMsg+''})                                        
                        }
                    })
                }
            })
            
        },
        getOpinionList(applyNo){ //获取发起人意见
            var getData={};
            getData.url = 'mobile.getOpinionInfoByApplyNo';
            getData.params = {
                applyNo:applyNo,
                operno: this.operNo,
                    };
            weexRequest.getRequestResult(getData,(res)=>{
                var con = JSON.parse(res.returnObject);
                if(res.successed){
                    this.initiatorOpinionList[0].content=con.descInfo;
                    this.initiatorOpinionList[1].content=con.regDate+' '+con.regTime;
                    if(con.fileDownloads.length==0){
                    this.initiatorOpinionList=this.initiatorOpinionList.slice(0,2)
                    }else{
                    this.initiatorOpinionList[2].content=con.fileDownloads;
                    this.initiatorOpinionList[3].content=con.customId;
                    }
                    
                    var obj={
                        regOperName:con.customId,
                        descType:con.descType,
                        descFlag:1,
                        show:true,
                        isShowDocFiles:'true',
                        regDate:con.regDate,
                        regTime:con.regTime,
                        descInfo:con.descInfo,
                        files:[{
                            descInfo:'123.pdf'
                        }]
                    }
                    this.opinions.unshift(obj);                 // this.opinions=con.risks;
                }else{
                    modal.toast({message:res.exceptionMsg+''})                                        

                }
            })
        },
        getDetailByApply(applyNo){
            var getData={};
            getData.url = 'mobile.applyDetail';
            getData.params = {
                applyNo:applyNo,
                operno: this.operNo,
                    };
            weexRequest.getRequestResult(getData,(res)=>{
                var con = JSON.parse(res.returnObject);
                this.reqDetailObj=con;
                if(res.successed){
                    this.buttonText='提交用印';
                    this.showBottmomBtn=true;
                    

                }else{
                    modal.toast({message:res.exceptionMsg+''})                                        
                }
            })
        },
        getAllFileInfoByApply(applyNo){ //获取当前相关联的文本
            var getData={};
                getData.url = 'mobile.getAllFileInfoByApplyNo';
                getData.params = {
                    applyNo:applyNo,
                    operno: this.operNo,
                     };
            weexRequest.getRequestResult(getData,(res)=>{
                var con = res.returnObject;
                this.DetailLists[0].content=con[0].sendSection;
                this.DetailLists[1].content=con[0].content;
                this.DetailLists[2].content=con[0].remark;
                var useArr=con.slice(1)
                if(res.successed){
                    if(useArr.length>0){
                        this.showFileList=useArr;
                    }
                }else{
                modal.toast({message:res.exceptionMsg+''})                                        

                }
            })
        },
        getCirculationOpinions(applyNo){
            var getData={};
                getData.url = 'mobile.getApprovalOpinionInfo';
                getData.params = {
                    applyNo:applyNo,
                     };
            weexRequest.getRequestResult(getData,(res)=>{
                var con = res.returnObject;
                if(res.successed){
                    this.opinions=con
                }else{
                modal.toast({message:res.exceptionMsg+''})                                        

                }
            })
        },
        openOpinion(){ //审批
            if(this.reqDetailObj.result.jngLowaContractApplyDTO.state=='00'){
                this.showOpinion = true;
            }else{
                navigatorPushEventFull('toDoSealApprovalSubmit');
            }
            
        },
        chooseButton(){
            var getData = {};
            getData.url='mobile.docAdd';
            getData.params = {
            customId: this.userInfoObj.persName,
            descFlag:this.buttonResult?'1':'3',
            applyNo:this.applyNo,
            regOper:this.operNo, 
            descType:'8',
            descInfo:this.output?this.output:'审批人未填写意见'
            }
            weexRequest.getRequestResult(getData, (res) => {
              var con = res;
              if(con.successed){
                    this.back();

                // setTimeout(()=>{
                //     //modal.toast({message: '审批成功'});
                // },1000)
              }else{
                  //modal.toast({message: '网络请求错误'});
              }
            })
        },
        back(){
            navigatorPopEvent(); 
        },
        filePreview(num,file){ //文件预览
            var serialNo='';
            var descType='';
            
            var filePath = ''
            if (weex.config.bundleUrl.split('/').slice(0,-2).join('/')=='http://60.191.59.19:10084/emas-bucket-public/app'){
              filePath =  'http://test.hccb.cc:7443'
            } else {
                filePath = 'https://api-emas.hzbank.com.cn'
            }
            // var fileURL = '';
            if(num==1){
                serialNo=file.serialNo;
                fileURL = filePath+'/HarmonySon/downloaddetail/loan?fileSerialNo='+serialNo+'&flag=formatbatchtwo&operno='+this.operNo+'&applyNo='+this.applyNo;
            }else{
                serialNo=file.fileSerialNo;
                descType=7;
                fileURL = filePath+'/HarmonySon/downloaddetail/loan?fileSerialNo='+serialNo+'&descType='+descType+'&flag=formatbatch&operno='+this.operNo;

            }
            weex.requireModule('commonEvent').webPreview(encodeURI(fileURL),res=>{});
        },
        openOpinion(){ //审批
                this.showOpinion = true;
            
        },
        closeOpinion(){ //关闭
            this.showOpinion = false;
        },
        agree(flag){  
            if(flag===1){
                this.descFlag = '1';
                this.output = '同意'
                this.buttonResult=true;
            }else if(flag===2)
            {
                this.descFlag2 = '1';
                this.buttonResult2=true;
            }         
        },
        veto(flag){
            if(flag===1){
                this.descFlag = '3';
                this.output = '';
                this.buttonResult=false; 
            }else if(flag===2){
                this.descFlag2 = '3';
                this.buttonResult2 = false; 
            }
        },
        cancel(){
            this.showDialog=false;
        },
       
    },
    computed:{
        opinionLength(){ 
            console.log(this.opinions.length);
            return this.opinions.length;       
        },
        value(){
            if(this.index<this.items.length){
                return this.items[this.index];
            }
            return '流转'
        }
    }
}
</script>

<style scoped>
.blueFont{
    color: rgb(0,164,234);
}
.submit3{
    position: fixed;
    left: 30px;
    bottom: 20px;
    width: 330px;
    height: 90px;
    border-radius: 45px;
    background-image: linear-gradient(to right, #3BC2FC, #1BA5E7);
    /* margin-top: 64px; */
    align-items: center;
    justify-content: center;
    /* margin-left: 30px; */
}
.submit4{
    position: fixed;
    left: 390px;
    bottom: 20px;
    width: 330px;
    height: 90px;
    border-radius: 45px;
    background-color: #fff;
    border-width:0.5px;
    border-color:rgb(0,164,234);
    color:rgb(0,164,234);
    /* margin-top: 64px; */
    align-items: center;
    justify-content: center;
    /* margin-left: 30px; */
}
.btnbg{
    height: 110px;
    background-color: #fff;
}
.buttonTextCancel{
    font-family: PingFangSC-Regular;
    font-size: 36px;
    color: rgb(0,164,234);
    text-align: center;
}
.certificatesBlock{
    /* height: 710px; */
    background-color: #fff;
    padding:32px;
    margin-bottom: 8px;
}
.blockTitle{
    height: 102px;
    line-height: 102px;
    font-family: PingFangSC-Medium;
}
.blockText{
    font-size: 28px;
    color: #333333;
    letter-spacing: 0;
    font-weight: 500;
}
.blockName{
    height: 40px;
    line-height: 40px;
}
.blockNameText{
    font-size: 28px;
    color: #999999;
}
.blockNameContent{
    font-family: PingFangSC-Regular;
}
.typeInfo{
    padding: 32px 0px 32px 0px;
    flex-direction: row;
    justify-content: space-around;
    border-top-color:#eeeeee;
    border-bottom-color:#eeeeee;
    border-top-width:1px;
    border-bottom-width:1px;
}
.typeInfo1{
    padding-left: 0px;
}
.typeInfoText{
    font-family: PingFangSC-Regular;
    font-size: 28px;
    text-align:left;
    width: 340px;
}
.typeInfoTitle{
    color: #999999;
    margin-bottom: 8px;
}
.typeInfoContent{
    color: #333333;
}

.wordItem{
    height: 180px;
    color: #999999;
  }
.wordItemTitle{
    height: 80px;
    line-height: 80px;
}
.wordItemText{
    color: #999999;
    font-size: 28px;
    height: 80px;
    line-height: 80px;
    vertical-align: top;
    margin: 0px;
    /* margin-left: 20px; */
}
  .wordItemContentIcon{
    width: 32px;
    height: 44px;
  }
  .wordItemShow{
    height: 100px;
    line-height: 100px;
    margin: 0px;
    flex-direction: row;
    justify-content: flex-start;
    align-items:center;
  }
  .wordItemContentText{
    padding-left: 20px;
    color: #999999;
    font-size: 28px;
    color: #00A4EA;
  }
  .wordItemTop{
    /* border-bottom-style: solid;
    border-bottom-width:1px;
    border-bottom-color:#cccccc; */
  }
.detail{
    background-color: #f8f8f8;
}
.header{
    padding-top: 40px;
    padding-bottom: 16px;
    flex-direction: row;
    justify-content: space-between;
}
.TitleText{
    font-family: PingFangSC-Regular;
    font-size:24px;
    color: rgb(153,153,153);
    margin-left: 30px;
}
.DetailCell{
    flex-direction: row;
    justify-content: space-between;
    padding-right: 32px;
    align-items: space-between;
    background-color: #fff
    
}
.contentText{
    font-family: PingFangSC-light;
    font-size:24px;
    color: rgb(51,51,51);
    margin-left: 40px;
}
.blank1{
    background-color: #fff;
    height: 48px;
}
.blank2{
    background-color: #fff;
    height: 32px;
}
.fileCell{
    background-color: #fff;
}
.fileType{
    padding-top: 24px;
    padding-bottom: 0px;
    padding-left: 90px;
    background-color: #fff;
}
.typeName{
    font-family: PingFangSC-light;
    font-size: 24px;
    color: rgb(153,153,153);
}
.file{
    height: 88px;
    border-bottom-width: 0.5px;
    border-bottom-color: rgb(153,153,153);
    margin-right: 30px;
    flex-direction: row;
    align-items: flex-start;
}
.margin-left{
    margin-left: 78px;
}
.fileimg{
    height:64px;
    width: 64px;
    margin-left: 36px;
    margin-top: 20px;
}
.fileName{
    font-family: PingFangSC-light;
    font-size: 24px;
    color: rgb(0,164,234);
    margin-left: 8px;
    margin-top: 56px;
}
.opinionCell{
    background-color: #fff;
}
/* .left{
    width:56px;
    justify-content: center;
    align-items: center;
    margin-left: 24px;
    margin-bottom: 0px;
    margin-top:0px;
    border-width: 1px;
} */
.title{
    flex-direction: row;
    /* border-width: 1px; */
}
.circle{
    width: 80px;
    height: 80px;
    border-radius: 40px;
    background-color: rgb(27,165,231);
    margin-left: 24px;
    margin-right: 20px;
    /* margin-top:26px; */
    align-items: center;
    justify-content: center;
}
.name{
    color: #fff;
    font-family: PingFangSC-light;
    font-size: 20px;
    text-align: center;
   
}
.line{
    position: absolute;
    width:1px;
    border-right-color: rgb(0,164,234);
    border-right-width: 4px;
    border-right-style: dotted;
    /* border-width: 1px; */
    top: 0;
    bottom: 0;
    left: 62px;
}
.content{
    flex-direction: row;
    /* border-width: 1px; */
}
.state{
    margin-top: 0px;
    margin-left: 0px;
    flex-direction: row;
    align-items: flex-start;
}
.stateText{
    font-family: PingFangSC-light;
    font-size: 24px;
    color: rgb(153,153,153);
     margin-top: 26px;
    /* text-align: left; */
}
.block{
    width:608px;
    /* height:64px; */
    background-color: #f5f5f9;
    justify-content: space-between;
    border-radius: 5px;
    margin-left:118px;
    margin-top: 0px;
    margin-right: 30px;
    margin-bottom: 72px;
}
.firstLine{
    width: 608px;
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
    margin-top: 24px;
    /* border-width: 1px; */
}
.leftIcon{
  flex-direction: row;
  align-items: flex-start;

}
.icon{
    width: 32px;
    height: 32px;
    margin-left: 16px; 
    margin-top: 3px;
}
.approvalContent{
    margin-left: 16px;
    font-family: PingFangSC-light;
    font-weight: bold;
    font-size: 28px;
    color: rgb(51,51,51);
}
.Time{
    flex-direction: row;
    align-items: flex-end;
}
.date{
    font-family: PingFangSC-light;
    font-size: 24px;
    color: rgb(153,153,153);
    margin-right: 15px;
}
.time{
    font-family: PingFangSC-light;
    font-size: 24px;
    color: rgb(153,153,153);
    margin-right: 16px;
}
.SecondLine{
    margin-left: 64px;
    margin-bottom: 16px;
    margin-top: 16px;
    
}
.result{
    font-family:  PingFangSC-light;
    font-size: 24px;
    color: rgb(102,102,102);
}
.button{
  background-color: #fff;
  margin-top:40px;
  height:68px;
  flex-direction: row;
  align-items: center;
}
/* .buttonLeft{
  width: 160px;
  height:40px;
  border-right-width: 0.5px;
  border-right-color: rgb(159,159,159);
} */
/* .buttonRight{
  width:160px;
} */
.buttonContent{
  font-family:  PingFangSC-light;
  font-size: 30px;
  text-align: center;
}
.approvalBox{
    background-color: #fff;
}
.dialogInput{
    margin-top: 20px;
    margin-left: 20px;
    margin-bottom: 20px;
    height: 200px;
    font-family: PingFangSC-light;
    font-size: 28px;
    background-color: rgb(248,248,248);
    border-color: rgb(248,248,248);
    border-width: 1px;
}
.editText{
    font-family: PingFangSC-light;
    font-size: 24px;
    color:rgb(0,164,234);
    margin-top: 26px; 
    margin-left:350px;
}
.deleteText{
    font-family: PingFangSC-light;
    font-size: 24px;
    color:rgb(0,164,234);
    margin-top: 26px;
    margin-left: 30px;
}
.buttons{
    flex-direction: row;
    justify-content: space-between;
}
.button{
    height: 64px;
    width: 160px;
    border-color: rgb(0,164,234);
    /* background-color: #fff; */
    border-width: 2px;
    border-radius: 32px;
    /* margin-top: 30px; */
    align-items: center;
    justify-content: center;
}
.left{
    margin-left: 80px;
}
.right{
    margin-right: 80px;
}
.left1{
    margin-left: 100px;
}
.right1{
    margin-right: 100px;
}
.buttonContent{
    font-family: PingFangSC-light;
    font-size: 36px;       
}
.saveOpinion{
    font-family: PingFangSC-light;
    font-size: 30px;
    color: rgb(0,164,234);
    text-align: right;
    margin-right: 24px; 
    font-weight: bold;
}
.submit{
    position: fixed;
    left: 30px;
    bottom: 20px;
    width: 690px;
    height: 90px;
    border-radius: 45px;
    background-image: linear-gradient(to right, #3BC2FC, #1BA5E7);
    /* margin-top: 64px; */
    align-items: center;
    justify-content: center;
    /* margin-left: 30px; */
}
.submit2{
    width: 690px;
    height: 90px;
    border-radius: 45px;
    background-image: linear-gradient(to right, #3BC2FC, #1BA5E7);
    margin-top: 64px;
    align-items: center;
    justify-content: center;
    margin-left: 30px;
    margin-bottom: 50px;
}
.buttonText{
    font-family: PingFangSC-Regular;
    font-size: 36px;
    color: #fff;
    text-align: center;
}
.dialogBox{
    position: fixed;
    left: 96px;
    /* width: 558px; */
    background-color: #FFFFFF;
    border-radius: 20px;
    top:400px;
}
.dialogContent{
    border-bottom-width: 1px;
    border-bottom-color: rgb(159,159,159);
    width: 558px;
    /* height: 400px; */
}
.dialogTitle{
  font-family:  PingFangSC-light;
  font-size: 30px;
  text-align: center;
  margin-top: 20px;
  color: rgb(51,51,51);
  font-weight: bold;
}
.dialogInput2{
    margin-left: 30px;
    margin-right: 30px;
    margin-top: 38px;
    margin-bottom: 30px;
    border-width: 1px;
    border-style: solid;
    border-color: rgb(159,159,159);
    border-radius: 10px;
    font-family: PingFangSC-light;
    font-size: 24px;
    color: rgb(51,51,51);
    height: 200px;
    width: 498px;

}
.prompt{
    width: 558px;
    height: 80px;
    flex-direction: row;
}
.buttonCancel{
    width:279px;
    border-right-width: 1px;
    border-right-color: rgb(159,159,159);
    align-items: center;
    justify-content: center;
}
.buttonComfirm{
    width: 279px;
    align-items: center;
    justify-content: center;
}
.cancel{
    font-family:  PingFangSC-light;
    font-size: 24px;
    color: rgb(159,159,159);
}
.comfirm{
    font-family:  PingFangSC-light;
    font-size: 24px;
    color: rgb(0,164,234);
}
.opinionInput{
    background-color: #fff;
    width: 750px;
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
}
.head{
    justify-content: space-between;
    flex-direction: row;
    padding-top: 40px;
}
.closeIcon{
    height: 64px;
    width: 64px;
    margin-right: 30px;
}
.opinionDialog{
    height: 304px;
    margin-left: 30px;
    margin-right: 30px;
    background-color: rgb(248, 248, 248);
    border-width: 1px;
    border-color: rgb(203,203,203);
    border-radius: 3px;
    margin-bottom: 30px;
    margin-top: 30px
}
.nodeList{
    background-color: #fff;
    height: 60px;
}
.node{  
    margin-right: 30px;
    margin-left: 30px;
    height: 60px;
    justify-content: center;
    border-bottom-width: 0.5px;
    border-bottom-color: rgb(203,203,203);
}
.node2{
    border-bottom-width: 1px;
    border-bottom-color: rgb(203,203,203);
    height: 60px;
    justify-content: center;
}
.nodeText{
    font-family:  PingFangSC-light;
    font-size: 28px;
    color: rgb(51,51,51);
}
.nodeText2{
    font-family:  PingFangSC-light;
    font-size: 28px;
    margin-left: 30px;
    color: rgb(51,51,51);
}
.TitleTextFZ{
    font-size:32px;
}
.TitleTextColor{
    color: rgb(153,153,153);
}
</style>
