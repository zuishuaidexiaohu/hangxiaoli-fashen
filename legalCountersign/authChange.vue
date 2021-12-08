<template>
    <div class="wapper">
        <oaHeader :title ="title" :leftIcon="backImage" :iconLeftWidth= "iconLeftWidth" :iconLeftHeight = "iconLeftHeight" v-on:clickEventLeft="back"></oaHeader>
        <div class="searchBox">
            <div class="imageBox">
                <image class="searchImg" :src="searchIcon"></image>
            </div>
           <div class="searchContent"  placeholder="操作员号或者姓名" >
               <text class="searchText" @click="search">全行搜索（用户名、手机号、短号,座机）</text>
           </div>
           <!-- <text class="searchButton" @click="search">搜索</text> -->
        </div> 
        <div class="listcell" v-if="showResult">
            <div class="cell">
                <text class="operNo">{{this.searchOper}}</text>
                <!-- <text class="name">{{authOper.persName}}</text>
                <text class="name">{{authOper.orgFullName}}</text> -->
            </div>
        </div> 
        <!-- </list> -->
        <div class="submit" @click="submit">
            <text class="buttonText">提交</text>
        </div>  
    </div>
</template>
<script>
import oaHeader from '../../component/oaHeader'
import { navigatorPopEvent, navigatorPushEvent } from '../../../utils/navigator'
import { weexRequest } from '../../../utils/weexAxios'
const storage = weex.requireModule('storage')
const modal=weex.requireModule('modal')
const navigator = weex.requireModule('navigator')
const sendUpdateFlag = new BroadcastChannel('update')
const getOper = new BroadcastChannel('getOper')

export default {
    components:{
        oaHeader
    },
     mounted() {
        storage.getItem('setPersonalMessage',  (event)=> {
            this.userInfoObj=JSON.parse(event.data);
            this.operIndexNo =this.userInfoObj.operNo?this.userInfoObj.operNo:'000844';
            this.operId = this.userInfoObj.operNo?this.userInfoObj.operNo:'000844';
            storage.getItem('toDoDetailObj',event=>{
            this.indexDetailObj=JSON.parse(event.data);
            this.applyNo=this.indexDetailObj.applyNo;
            }) 
        }); 
       getOper.onmessage = event=>{
           var userInfo = JSON.parse(event.data);
           this.showResult = true;
          // this.authOperList.push(userInfo);
          this.authOper = userInfo;
          this.operNo = this.authOper.operno;
          this.name = this.authOper.persName; 
          this.searchOper = this.operNo+'-'+this.name+'('+this.authOper.orgFullName+')'
         // modal.alert({message:event.data});
       }
    },
    data(){
        return{
            title:'文本会签',
            backImage:'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABUAAAAkCAYAAABmMXGeAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAAAZdEVYdFNvZnR3YXJlAEFkb2JlIEltYWdlUmVhZHlxyWU8AAADKGlUWHRYTUw6Y29tLmFkb2JlLnhtcAAAAAAAPD94cGFja2V0IGJlZ2luPSLvu78iIGlkPSJXNU0wTXBDZWhpSHpyZVN6TlRjemtjOWQiPz4gPHg6eG1wbWV0YSB4bWxuczp4PSJhZG9iZTpuczptZXRhLyIgeDp4bXB0az0iQWRvYmUgWE1QIENvcmUgNS42LWMwNjcgNzkuMTU3NzQ3LCAyMDE1LzAzLzMwLTIzOjQwOjQyICAgICAgICAiPiA8cmRmOlJERiB4bWxuczpyZGY9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkvMDIvMjItcmRmLXN5bnRheC1ucyMiPiA8cmRmOkRlc2NyaXB0aW9uIHJkZjphYm91dD0iIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtbG5zOnhtcE1NPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvbW0vIiB4bWxuczpzdFJlZj0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL3NUeXBlL1Jlc291cmNlUmVmIyIgeG1wOkNyZWF0b3JUb29sPSJBZG9iZSBQaG90b3Nob3AgQ0MgMjAxNSAoTWFjaW50b3NoKSIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDo1QTExREM0MjREMUIxMUU4OEY4OEY2NDBGOTMyMzY0NiIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDo2QzZEMjY1RTREMUYxMUU4OEY4OEY2NDBGOTMyMzY0NiI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOjVBMTFEQzQwNEQxQjExRTg4Rjg4RjY0MEY5MzIzNjQ2IiBzdFJlZjpkb2N1bWVudElEPSJ4bXAuZGlkOjVBMTFEQzQxNEQxQjExRTg4Rjg4RjY0MEY5MzIzNjQ2Ii8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+uGGeXQAAAmhJREFUSEullsuLjWEcx5+ZE8WCXFZnWLvt3akxFAqFUu7EyZIZMwv/gQwTK52IYcFGUeRSLNxjyYYdZSWXDcrE8fn+nvc53nN9b5/69M7vXXzn6Xmf3+85PbVazTVTrVajv1IzGc/iTjzfqzcFmYq38AhOw8GiobPxEa63ynOjSOgcfIJLrPJcwT15QxfiS5xvlecM7seJPKHLUCvss8ozjENoXz1r6EZ8iDOtcu4PHsBRqyKyhO7FmzjFKud+4Ra8bFWMtKGDOI6TrHLuG67FO1Y1kRTag6fwtFWeT7gan1vVhm6hJbyEx63yvMPl+NaqDnQKDV2yzyrPK1yFH63qQkvv0/czeNxGrShwH7fiT6sSaFgpgaFL4oHXcDOmChT1UALn8dDmL7IXHk2eXfjbqpRYKIGLeTzFuaojTuBRbJ2NCZTK5fI6nvdwur1x7i8exnNW5UArvY762kJdsg0vWpWTdkdqInrmRqG6ArRCob7W+dQIy01vpVLRGVyDX+3N/07SOMtF/fBzAjR4H2B8To5hfU6mpaGjosOv4AX2wqMr4hCm3ut2bTqLh9p0qb3w3MXtmL1NBXv8hccA6uwGNqAmvv5hIu2OlNCKNNWvWuXRyh+jtqgrnUKF+l2jTx8roI/5AuN73kK3UKEN11UyYpUnTLL4njeQFBrQlXIQNReE9la/TLTXLaQNFWoI7XNz9+22KkaWUKGjpqn23Sp/u+pjHrMqImuoeIa6TXWrBvST5yTq9s0VKt7gCnxvlUcfUyOzlDdUfMCV+Noqj34CjRcJFZ+xHzUvAjuKhoofuAkvICfDjf0Djb6D75MHfhwAAAAASUVORK5CYII=',
            iconLeftHeight:'40px',
            iconLeftWidth:'25px',
            searchIcon:'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACoAAAAqCAYAAADFw8lbAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyhpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMDY3IDc5LjE1Nzc0NywgMjAxNS8wMy8zMC0yMzo0MDo0MiAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTUgKE1hY2ludG9zaCkiIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6MTU5QUY5OTQ1MEQ0MTFFOEI4OUJFNjM5Q0EyMDZBNzQiIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6MTU5QUY5OTU1MEQ0MTFFOEI4OUJFNjM5Q0EyMDZBNzQiPiA8eG1wTU06RGVyaXZlZEZyb20gc3RSZWY6aW5zdGFuY2VJRD0ieG1wLmlpZDoxNTlBRjk5MjUwRDQxMUU4Qjg5QkU2MzlDQTIwNkE3NCIgc3RSZWY6ZG9jdW1lbnRJRD0ieG1wLmRpZDoxNTlBRjk5MzUwRDQxMUU4Qjg5QkU2MzlDQTIwNkE3NCIvPiA8L3JkZjpEZXNjcmlwdGlvbj4gPC9yZGY6UkRGPiA8L3g6eG1wbWV0YT4gPD94cGFja2V0IGVuZD0iciI/PpvcFXUAAAOySURBVHjazJl7aE5hHMfPa5eYjQy5hoyokeUdtVrMXdFcZkiay9ay8Je7KPHfFiJGmFCLZmRsrrWVhWjDSjaGWGtJq2EXMjbfX/u99XQ67znPc95z3ne/+rTfOe/lfN7nPM9znt8zT1VVlaYQg8BiMAfEgwkgGkSANtAI6sBTcJ9z0/B6vVIXDpcUnAl2gmUg0uRHEFNBOjgOqBVOgqvgrxZA9LF4fSy4CZ7zxUXJdvAKPAJ3uRW/6j6fCK6AN2BBIKJmLboKFIABwrnP4DIoATWgy+Bzo8EikAFm8blJ4CE4BXaAP0616AFwXZBsAhvBRHCIW7LLz2cb+QfOBjNAhfDaNu67A50QPQiOCMeFYDK3pGo/oz46D2SD33yOBuI9EBWI6DpwWDjeA9aD1gC6Vzc4zy3czOeSuO/aEo0D54RjGuW5mnPxggfUTz5OA1vsiFJH78853eajmvPxmu+QL3Krq6tHqIim8ERO0QC2au7FHeHOxYD9KqJ7df2yXXM36Ho/OM9Cqw6WER0lTMbvQZHmfrSAs5z3BWtkRJcKLVtgMj86HQVCvlxGNEXXf4IV9aCW82Tc/nAr0XjhdtRqwY0n/LcfGGcl6nvDOy34Ua+bx01FYzhvDoHoNyGPlp3wu7TQRpiVaAfnQ0IgFyvkrVaiDTJ9xKUQr/nRSvQt58OsRp4LkcR/22REK4XjJUGUpCdiAucVKPL+WYmWCsebgyi6AXg4vyHzZPrAhRnFdGEV5WbQBL+d8+9c9kitnvKEc3kKZbTdoJpsOOcncNs7ZEVLeAVOMUVXjrgxgHYLRWOeynqU6pocoXjbB9a6IDkGFAt3LBut2a4iSvFSt4Cm4mu1w3NmORipE7dVhR4DFzin/aRrXDqHBSi5EDwzeKjkY3mXY0e0mytD36LWwx2f6vNkG4JUuF0ED8BQP++RkjXagKCJNwvsEvpsAj8YKnn+izX5TtqfmgsugS9gk8QPItlMszd4LLYdaUvmDPAatDwtsut4edip9ezkjQfTeJ4Uo4nvTAvXZBEG16LVWwYGV6EdUV+rp/OGRKLirf8ETnMh55srU3nkK8l6FDdyqWxZwdsz8dwHxaASmLYYH4MyfuJ1G3yPsqyqqD6ieGUeyZIqe1RWsmmQvWU2mFSig8uJRhsbabe1nj3YTj/drQgDLNUp0UDDTJZautgnG2pR+meDjOz8kIsKsivBLz+y+b1ClGVLeXvJSDau14iybLkf2bJeJSrI0iKmhmcSKlMy/wswAM+26JuhLGHnAAAAAElFTkSuQmCC',
            operNo:'',
            name:'',
            searchOper:'',
            authOperList:[],
            getResult:false,
            showResult:false,
            buttonResult:'',
            applyNo:'',
            authOper:{},
            userInfoObj:{},//当前用户信息
            indexDetailObj:{},//当前数据基础信息
            operIndexNo:''
        }
    },

    methods:{
        back(){
            navigatorPopEvent();
        },
        submit(){
            storage.getItem('button',event=>{
                 
                    this.buttonResult = event.data;
                    switch(this.buttonResult){
                        case 'cf_btn_change':
                            this.change();
                        break;
                        case 'cf_btn_supplyPartChange':
                            this.supplyPartChange();
                        break;
                        case 'cf_btn_judgeChange':
                            this.judgeChange();
                        break;
                    }
                })
        },
        change(){
            if(this.operNo==''){
                modal.alert({message:'您还没选转授权人员'});
                return;
            }
            modal.confirm({message:'确认转授权',
                okTitle:'确认',
                cancelTitle:'取消'},
                result=>{
                    if(result==='确认'){
                        var authChangeData={}
                        authChangeData.url='mobile.legalCountersignChange'
                        authChangeData.params={operId:this.operId,applyNo:this.applyNo,authOper:this.operNo}
                        weexRequest.getRequestResult(authChangeData, (res) => {
                            var con = res;
                            if(con.successed){
                                if(con.exceptionMsg==null){
                                    modal.toast({message:'转授权成功'})
                                    var obj={route:1}
                                    storage.setItem('routeObj', JSON.stringify(obj), event=>{
                                    navigatorPopEvent();   
                                    });
                                }else{
                                    modal.toast({message:con.exceptionMsg+''})
                                }
                            }else{
                                    modal.toast({message:con.exceptionMsg+''})                                        
                            }
                        }) 
                    }else{
                       
                    }
                })   
        },
        
        supplyPartChange(){
            if(this.operNo==''){
                modal.alert({message:'您还没选转授权人员'});
                return;
            }
            modal.confirm({message:'确认转授权',
                okTitle:'确认',
                cancelTitle:'取消'},
                result=>{
                    if(result==='确认'){
                        var supplyPartChangeData={}
                            supplyPartChangeData.url='mobile.legalCountersignSupplyPartChange'
                            supplyPartChangeData.params={'operId':this.operId,'applyNo':this.applyNo,'authOper':this.operNo}
                            weexRequest.getRequestResult(supplyPartChangeData, (res) => {
                                var con = res;
                                if(con.successed){
                                    if(con.exceptionMsg==null){
                                        sendUpdateFlag.postMessage('1');
                                        modal.toast({message:'转授权成功'});
                                        var obj={route:1}
                                        storage.setItem('routeObj', JSON.stringify(obj), event=>{
                                        navigatorPopEvent();   
                                        });

                                    }else{
                                        modal.toast({message:con.exceptionMsg+''})
                                    }
                                }else{
                                    modal.toast({message:con.exceptionMsg+''})                                        
                                }
                            })
                    }else{
                      
                    }
                })
        },
        judgeChange(){
            if(this.operNo==''){
                modal.alert({message:'您还没选转授权人员'});
                return;
            }
            modal.confirm({message:'确认转授权',
                okTitle:'确认',
                cancelTitle:'取消'},
                result=>{
                    if(result==='确认'){
                        var judgeChangeData={}
                        judgeChangeData.url='mobile.legalCountersignJudgeChange'
                        judgeChangeData.params={'operId':this.operId,'applyNo':this.applyNo,'authOper':this.operNo}
                        weex.requireModule('commonEvent').getRequestResult(JSON.stringify(judgeChangeData), (res) => {
                            var con = res;
                            if(con.successed){
                                if(con.exceptionMsg==null){
                                    // sendUpdateFlag.postMessage('1');
                                    modal.toast({message:'转会签成功'})
                                    var obj={route:1}
                                    storage.setItem('routeObj', JSON.stringify(obj), event=>{
                                    navigatorPopEvent();   
                                    });
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
        search(){
            this.authOperList=[];
            navigatorPushEvent("searchOper");
        }

    }
}
</script>

<style scoped>
.wapper{
    background-color: #f8f8f8;
}
.searchBox{
    background-color: #fff;
    flex-direction: row;
    justify-content: flex-start;
}
.imageBox{
    height: 68px;
    width: 68px;
    margin-top: 30px;
    margin-left: 20px;
    background-color: #f8f8f8;
    align-items: center;
    justify-content: center
}
.searchImg{
    height: 28px;
    width: 28px;
}
.searchContent{
    background-color: #f8f8f8;
    border-radius: 5px;
    font-size: 28px;
    height: 68px;
    margin-right: 20px;
    width: 640px;
    /* margin-left: 20px; */
    margin-top: 30px;
    margin-bottom: 30px;
}
.searchText{
    font-family: PingFangSC-light;
    text-align: left;
    color:  rgb(153,153,153);
    font-size: 26px;
    line-height:64px;
}
.searchButton{
    font-family: PingFangSC-light;
    font-size: 30px;
    color:rgb(0,164,234);
    margin-top: 40px;
    margin-left: 20px;
}
.searchResult{
    background-color: #fff
}
.listcell{
    background-color: #fff;
    width: 750px;   
}
.cell{
    background-color: #fff;
    margin-left: 30px;
    margin-right: 30px;
    height: 68px;
    border-bottom-width: 0.5px; 
    flex-direction: row;
    align-items: center;
    justify-content: flex-start;
    width: 640px;
}
.operNo{
    font-family: PingFangSC-light;
    font-size: 28px;
    margin-left: 20px;
    width: 600px;

}
.name{
    font-family: PingFangSC-light;
    font-size: 28px;
    margin-left: 30px;
}
.submit{
    width: 690px;
    height: 90px;
    border-radius: 45px;
    background-image: linear-gradient(to right, #3BC2FC, #1BA5E7);
    margin-top: 64px;
    align-items: center;
    justify-content: center;
    margin-left: 30px;
}
.buttonText{
    font-family: PingFangSC-Regular;
    font-size: 36px;
    color: #fff;
    text-align: center;
}

</style>
