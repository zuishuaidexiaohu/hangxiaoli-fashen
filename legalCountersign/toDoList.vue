<template>
  <div  class="addressBook" @viewappear='toRefresh'>
    <fixed></fixed>
    <oaHeader  :leftIcon="backImage" :iconLeftWidth= "iconLeftWidth" :iconLeftHeight = "iconLeftHeight" v-on:clickEventLeft="back" title="待办任务列表"   
   ></oaHeader>
    <div class="wap" v-if="!isBlank">
      <scroller class="scroller" >
        <refresh class="refresh" @refresh="onrefresh" :display="refreshing ? 'show' : 'hide'">
          <loading-indicator class="indicator"></loading-indicator>
        </refresh>
        <div @click="toDetail(item)" v-for="(item,index) in toDoList" :key="index" class="blockItem">
          <!-- 格式批量代办 -->
          <div v-if="item.type=='apply'">
            <div >
              <text class="toDoTitle">{{item.jngLowaContractApplyDTO.applyName}}</text>
            </div>
            <div class="toDoContent">
              <text class="toDoText">业务类型：</text> 
              <text class="toDoText">格式批量待办</text>
            </div>
            <div class="toDoContent">
              <text class="toDoText">申请人：</text>
              <text class="toDoText">{{item.jngLowaContractApplyDTO.regOper}}</text>
            </div>
            <div class="toDoContent">
              <text class="toDoText">申请时间：</text>
              <text class="toDoText">{{item.jngLowaContractApplyDTO.regDate}}</text>
            </div>
            <div class="toDoContent">
              <text class="toDoText">申请编号：</text>
              <text class="toDoText">{{item.jngLowaContractApplyDTO.applyNo}}</text>
            </div>
            <div class="toDoContent">
              <text class="toDoText">流程状态：</text>
              <text class="toDoText">{{item.jngLowaContractApplyDTO.state}}</text>
            </div>
          </div>
          <!-- 会签 -->
          <div v-if="item.type=='isFW'||item.type=='notFW'">
            <div >
              <text class="toDoTitle">{{item.bsgLowaApplyRegDTO.fileName}}</text>
            </div>
            <div class="toDoContent">
              <text class="toDoText">业务类型：</text> 
              <text class="toDoText">法律文本审批待办</text>
            </div>
            <div class="toDoContent">
              <text class="toDoText">申请人：</text>
              <text class="toDoText">{{item.bsgLowaApplyRegDTO.regOper}}</text>
            </div>
            <div class="toDoContent">
              <text class="toDoText">申请时间：</text>
              <text class="toDoText">{{item.bsgLowaApplyRegDTO.regDate}}</text>
            </div>
            <div class="toDoContent">
              <text class="toDoText">申请编号：</text>
              <text class="toDoText">{{item.bsgLowaApplyRegDTO.applyNo}}</text>
            </div>
            <div class="toDoContent">
              <text class="toDoText">流程状态：</text>
              <text class="toDoText">{{item.bsgLowaApplyRegDTO.stateMapName}}</text>
            </div>
          </div>
           <!-- 格式批量待审批 -->
          <div v-if="item.type=='approval'">
            <div >
              <text class="toDoTitle">{{item.bsgLowaContractApprovalDTO.applyName}}</text>
            </div>
            <div class="toDoContent">
              <text class="toDoText">业务类型：</text> 
              <text class="toDoText">格式批量待审批</text>
            </div>
            <div class="toDoContent">
              <text class="toDoText">申请人：</text>
              <text class="toDoText">{{item.bsgLowaContractApprovalDTO.regOper}}</text>
            </div>
            <div class="toDoContent">
              <text class="toDoText">申请时间：</text>
              <text class="toDoText">{{item.bsgLowaContractApprovalDTO.regDate}}</text>
            </div>
            <div class="toDoContent">
              <text class="toDoText">申请编号：</text>
              <text class="toDoText">{{item.bsgLowaContractApprovalDTO.applyNo}}</text>
            </div>
            <div class="toDoContent">
              <text class="toDoText">流程状态：</text>
              <text class="toDoText">{{item.bsgLowaContractApprovalDTO.approvalStateShow}}</text>
            </div>
          </div>
          <!-- 用印数据 -->
          <div v-if="item.type=='flow'">
            <div >
              <text class="toDoTitle">{{item.bsgLowaApplyPreFlowDTO.applyName}}</text>
            </div>
            <div class="toDoContent">
              <text class="toDoText">业务类型：</text> 
              <text class="toDoText">用印待办</text>
            </div>
            <div class="toDoContent">
              <text class="toDoText">申请人：</text>
              <text class="toDoText">{{item.bsgLowaApplyPreFlowDTO.regOper}}</text>
            </div>
            <div class="toDoContent">
              <text class="toDoText">申请时间：</text>
              <text class="toDoText">{{item.bsgLowaApplyPreFlowDTO.regDate}}</text>
            </div>
            <div class="toDoContent">
              <text class="toDoText">申请编号：</text>
              <text class="toDoText">{{item.bsgLowaApplyPreFlowDTO.applyNo}}</text>
            </div>
            <div class="toDoContent">
              <text class="toDoText">流程状态：</text>
              <text class="toDoText">{{item.bsgLowaApplyPreFlowDTO.taskState}}</text>
            </div>
          </div>
      </div>
      <loading class="loading" @loading="more" :display="loadinging ? 'show' : 'hide'">
          <!-- <text class="loadMoreTxt">正在加载更多</text> -->
          <loading-indicator class="indicator"></loading-indicator>
      </loading>
      </scroller>
    </div>
    
    <blank v-if="isBlank"></blank>
  </div>
</template>

<style scoped>
  .loading {
      padding: 0 16px;
      width: 750px;
      align-items: center;
    }
    .indicator {
      margin-top: 16px;
      height: 40px;
      width: 40px;
      color: rgb(153,153,153);
    }
  /* 根页面 */
  .addressBook {
    padding-bottom: 0;
    background-color: #f5f5f9;
    /* background: #F5F5F9; */
  }
  .wap {
    position: absolute;
    top: 150px;
    right: 0;
    bottom: 0;
    left: 0;
  }
   .scroller {
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: 9;
  }
  .blockItem{
    margin-bottom: 16px;
    height:378px;
    background-color: #ffffff;
    /* background: #FFFFFF; */
    /* margin-bottom: 32px; */
    padding:32px 0px 0px 32px;
  }
  .toDoTitle{
    font-size: 32px;
    color: #333333;
    letter-spacing: 0;
    font-weight: 500;
    height: 40px;
  }
  .toDoContent{
    font-family: PingFangSC-Regular;
    letter-spacing: 0;
    font-weight: 400;
    flex-direction: row;
    align-items: flex-end;
    margin: 12px 0px;
  }
  .toDoText{
    color: #999999;
    font-size: 28px;
    margin-top: 0px;
  }
  .refresh {
    width: 750;
    display: -ms-flex;
    display: -webkit-flex;
    display: flex;
    -ms-flex-align: center;
    -webkit-align-items: center;
    -webkit-box-align: center;
    align-items: center;
  }
  .indicator {
    margin-top: 16px;
    height: 40px;
    width: 40px;
    color: rgb(153, 153, 153);
  }
</style>

<script>
  import fixed from '@/views/component/fixed.vue'
  import oaHeader from '@/views/component/oaHeader.vue'
  import blank from '@/views/component/blank.vue'
  import { navigatorPushEvent, navigatorPopEvent,navigatorPushFullScreenEvent,navigatorPushEventurlNo,navigatorPushEventFull} from '@/utils/navigator.js'
  import { picPath } from '@/utils/picturePath'
  import { HanZi_PinYin } from '@/utils/pinyin'
  const animation = weex.requireModule("animation")
  const storage = weex.requireModule('storage')
  const modal = weex.requireModule('modal')
  const dom = weex.requireModule('dom')
  const navigator = weex.requireModule('navigator')
  import { weexRequest } from '../../../utils/weexAxios'
  export default {
    components: {
      fixed, oaHeader,blank
    },
    data () {
      return {
        backImage:'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABUAAAAkCAYAAABmMXGeAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAAAZdEVYdFNvZnR3YXJlAEFkb2JlIEltYWdlUmVhZHlxyWU8AAADKGlUWHRYTUw6Y29tLmFkb2JlLnhtcAAAAAAAPD94cGFja2V0IGJlZ2luPSLvu78iIGlkPSJXNU0wTXBDZWhpSHpyZVN6TlRjemtjOWQiPz4gPHg6eG1wbWV0YSB4bWxuczp4PSJhZG9iZTpuczptZXRhLyIgeDp4bXB0az0iQWRvYmUgWE1QIENvcmUgNS42LWMwNjcgNzkuMTU3NzQ3LCAyMDE1LzAzLzMwLTIzOjQwOjQyICAgICAgICAiPiA8cmRmOlJERiB4bWxuczpyZGY9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkvMDIvMjItcmRmLXN5bnRheC1ucyMiPiA8cmRmOkRlc2NyaXB0aW9uIHJkZjphYm91dD0iIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtbG5zOnhtcE1NPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvbW0vIiB4bWxuczpzdFJlZj0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL3NUeXBlL1Jlc291cmNlUmVmIyIgeG1wOkNyZWF0b3JUb29sPSJBZG9iZSBQaG90b3Nob3AgQ0MgMjAxNSAoTWFjaW50b3NoKSIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDo1QTExREM0MjREMUIxMUU4OEY4OEY2NDBGOTMyMzY0NiIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDo2QzZEMjY1RTREMUYxMUU4OEY4OEY2NDBGOTMyMzY0NiI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOjVBMTFEQzQwNEQxQjExRTg4Rjg4RjY0MEY5MzIzNjQ2IiBzdFJlZjpkb2N1bWVudElEPSJ4bXAuZGlkOjVBMTFEQzQxNEQxQjExRTg4Rjg4RjY0MEY5MzIzNjQ2Ii8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+uGGeXQAAAmhJREFUSEullsuLjWEcx5+ZE8WCXFZnWLvt3akxFAqFUu7EyZIZMwv/gQwTK52IYcFGUeRSLNxjyYYdZSWXDcrE8fn+nvc53nN9b5/69M7vXXzn6Xmf3+85PbVazTVTrVajv1IzGc/iTjzfqzcFmYq38AhOw8GiobPxEa63ynOjSOgcfIJLrPJcwT15QxfiS5xvlecM7seJPKHLUCvss8ozjENoXz1r6EZ8iDOtcu4PHsBRqyKyhO7FmzjFKud+4Ra8bFWMtKGDOI6TrHLuG67FO1Y1kRTag6fwtFWeT7gan1vVhm6hJbyEx63yvMPl+NaqDnQKDV2yzyrPK1yFH63qQkvv0/czeNxGrShwH7fiT6sSaFgpgaFL4oHXcDOmChT1UALn8dDmL7IXHk2eXfjbqpRYKIGLeTzFuaojTuBRbJ2NCZTK5fI6nvdwur1x7i8exnNW5UArvY762kJdsg0vWpWTdkdqInrmRqG6ArRCob7W+dQIy01vpVLRGVyDX+3N/07SOMtF/fBzAjR4H2B8To5hfU6mpaGjosOv4AX2wqMr4hCm3ut2bTqLh9p0qb3w3MXtmL1NBXv8hccA6uwGNqAmvv5hIu2OlNCKNNWvWuXRyh+jtqgrnUKF+l2jTx8roI/5AuN73kK3UKEN11UyYpUnTLL4njeQFBrQlXIQNReE9la/TLTXLaQNFWoI7XNz9+22KkaWUKGjpqn23Sp/u+pjHrMqImuoeIa6TXWrBvST5yTq9s0VKt7gCnxvlUcfUyOzlDdUfMCV+Noqj34CjRcJFZ+xHzUvAjuKhoofuAkvICfDjf0Djb6D75MHfhwAAAAASUVORK5CYII=',        
        iconLeftHeight:'40px',
        iconLeftWidth:'25px',
        refreshing: false,
        toDoList:[],//会签模块数组
        ismove: false,
        startX: '',
        scope:'',
        state:'',
        cellImgs: [ {
          img: picPath + 'organizational_structure.png',
          msg: '组织架构'
        },{ // 菜单栏图标和文字列表
          img: picPath + 'myOrganization.png',
          msg: '我的机构'
        }, { // 菜单栏图标和文字列表
          img: picPath + 'contacts.png',
          msg: '最近联系人'
        }],
        Env: WXEnvironment, // 获取设备环境变量
        teamImg: picPath + 'teamImg.png',
        operNo:'',//操作员号
        page:1,
        loadinging:false,//加载更多
        rows:'10',
        nowPage:'',
        nowPageSize:'',
        isBlank:false
        // results:[],
      }
    },
    watch: {

    },
    created(){
      this.scope = (750 / this.cellImgs.length);  
    },
    mounted () {
      storage.getItem('setPersonalMessage',  (event)=> {
        var userInfo = JSON.parse(event.data);
        this.operNo =userInfo.operNo?userInfo.operNo:'000844';
        this.search('1','10');
      });
        
    },
    methods: {
      back(){
            navigatorPopEvent(); 
        },
      toRefresh(){
        this.onrefresh()
      },
      more () {
        this.loadinging = true
        this.search(''+(Number(this.page)+1),'10');
      },
      onrefresh(event){
        this.refreshing = true;
        this.toDoList=[];
        this.nowPageSize=Number(this.rows)*Number(this.page);
        this.search('1',this.nowPageSize);
      },
      toDetail(obj){
          var url = weex.config.bundleUrl.split('/').slice(0,-1).join('/') + '/views/pages/';        
          storage.setItem('toDoDetailObj', JSON.stringify(obj), event=>{
            if(obj.type=='flow'){ //
              navigatorPushEventFull('toDoSealDetail');
            }else if(obj.type=='apply'  ){ //
              navigatorPushEventFull('toDoBatchSubmission');
            }else if(obj.type=='isFW' || obj.type=='notFW'){ //会签详情
              navigatorPushEventFull('toDoSealApproval');
            }else if(obj.type=='approval'){ // 批量详情审批 、
              navigatorPushEventFull('toDoBatchDetails');
            }
        });
      },
      search(page,pageSize){
            var timestamp=new Date().getTime();
            var getData = {};
            getData.url='mobile.todoHome';
            getData.params = {
            operno: this.operNo,
            filter_LIKES_applyNo:'',
            _search:'false',
            rows:pageSize, 
            nd:''+timestamp,
            page:''+page,
            sidx:'',
            sord:'asc'}
            weexRequest.getRequestResult(getData, (res) => {
              var con = JSON.parse(res.returnObject);
              // modal.alert({message:con})
              if(res.successed){
                this.toDoList=this.page==1?[]:this.toDoList;
                if(con.result.content.length === 0){
                    modal.toast({message: '未查询到相应的任务'});
                      this.loadinging = false;  //加载更多关闭  
                      
                }else{
                  this.toDoList.push(...con.result.content);
                    if(this.refreshing){
                      this.refreshing = false;  
                    }
                    if(this.loadinging){
                      this.loadinging = false;  //加载更多关闭   
                      this.page=''+(Number(this.page)+1);
                    }
                }
                if(this.toDoList.length==0){
                    this.isBlank=true;
                  }else{
                    this.isBlank=false;
                  } 
                var obj={type:0}
                storage.setItem('toDoRefresh', JSON.stringify(obj), event=>{

                });
              }else{
                modal.toast({message:res.exceptionMsg+''})                                        

              }
            })
                
        },
    },
    
    
  }
</script>

