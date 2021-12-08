<template>
  <div class="addressBook">
    <fixed></fixed>
    <oaHeader title="搜索人员"  :leftIcon="popIcon" iconLeftWidth="25" iconLeftHeight="40" v-on:clickEventLeft="jump"></oaHeader>

    <!-- 搜索框 -->
    <div class="search">
      <input type="text" @input="searchAddressBook"  autofocus=true class="searchInput" placeholder="全行搜索（用户名、手机号、短号、座机）">
    </div>

    <!-- 菜单栏 -->
    
    <!-- 水印 -->
    <div class="bgWaterMark">
        <div class="waterMark">
          <text class="waterMarkText">{{cccc}}</text>
          <text class="waterMarkText">{{cccc}}</text>
          <text class="waterMarkText">{{cccc}}</text>
        </div>
        <div class="waterMark">
          <text class="waterMarkText">{{cccc}}</text>
          <text class="waterMarkText">{{cccc}}</text>
          <text class="waterMarkText">{{cccc}}</text>
        </div>
        <div class="waterMark">
          <text class="waterMarkText">{{cccc}}</text>
          <text class="waterMarkText">{{cccc}}</text>
          <text class="waterMarkText">{{cccc}}</text>
        </div>
        <div class="waterMark">
          <text class="waterMarkText">{{cccc}}</text>
          <text class="waterMarkText">{{cccc}}</text>
          <text class="waterMarkText">{{cccc}}</text>
        </div>
        <div class="waterMark">
          <text class="waterMarkText">{{cccc}}</text>
          <text class="waterMarkText">{{cccc}}</text>
          <text class="waterMarkText">{{cccc}}</text>
        </div>
    </div>


    <!-- 通讯录主体页 -->
    <div class="session" >
      <!-- 常用联系人 -->
      <scroller class="address">
        <!-- 侧边搜索栏 -->
        <!-- <div  class="searchAside" ref="searchAside"> -->
          <!-- <text class="searchLetter" v-for="(letter, index) in searchLetters" :key="index" @click="scrollTo(letter)">{{letter}}</text> -->
          <!-- 点击侧边栏搜索字母在中间显示正在点击的字母 -->
          <!-- <text class="txtLetter" ref="txtLetter" :style="{'top' : txtLetterTop}">{{txtLetter}}</text> -->
        <!-- </div> -->
        <!-- 联系人明细列表 -->
        <div  class="addressInfo">
            <!-- 拼音首字母 -->
            <!-- <div class="addressTitle"  :ref="userItem.letter">
              <text class="titleLetter">{{userItem.letter}}</text>
            </div> -->
            <!-- 联系人列表 -->
            <div class="addressInfoList" v-for="(val, index) in userInfoDataList" :key="index" @click="choose(val)">
              <image class="avatarImg" :src="avatarImg" v-if="!true"></image>
              <div class="avatarTxt" v-if="true">
                <text class="avatarName">{{val.persName.slice(-2)}}</text>
              </div>
              <div class="info">
                <text class="name">{{val.persName}}</text>
                <text class="dept">{{val.orgFullName}}</text>
              </div>
            </div>
          <!-- <div class="addressInfoList" @click="JumpToDetail(letter + '白帆', '电子银行部', avatarImg)">
            <image class="avatarImg" :src="avatarImg" v-if="true"></image>
            <div class="avatarTxt" v-if="!true">
              <text class="avatarName">{{'白帆'}}</text>
            </div>
            <div class="info">
              <text class="name">{{letter + '白帆'}}</text>
              <text class="dept">电子银行部</text>
            </div>
          </div> -->
        </div>
       <loading class="loading" @loading="more" :display="loadinging ? 'show' : 'hide'">
            <!-- <text class="loadMoreTxt">正在加载更多</text> -->
            <loading-indicator class="indicator"></loading-indicator>
            </loading>
      </scroller>
    </div>
  </div>
</template>

<style scoped>
  /* 根页面 */
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
   .more {
      color: rgb(102,102,102);
      font-size: 40px;
      font-family: PingFangSC-Regular;
      margin-right: 16px;
      text-align: center;
    }
  .addressBook {
    padding-bottom: 15px;
    /* background-color: rgb(244,244,244); */
  }

  /* 搜索框 */
  .search {
    padding: 16px;
    background-color: rgb(244,244,244);
    justify-content: center;
    align-items: center;
  }
  .searchInput {
    width: 718px;
    height: 64px;
    background-color: rgb(255,255,255);
    border-radius: 8px;
    text-align: center;
    color: rgb(153,153,153);
    font-size: 26px;
  }

  /* 菜单栏 */
  .menu {
    flex-direction: row;
  }
  .menuCell {
    justify-content: center;
    align-items: center;
    flex: 1;
    height: 146px;
  }
  .cellImg {
    margin-top: 24px;
    width: 48px;
    height: 48px;
  }
  .cellMsg {
    font-size: 26px;
    color: rgb(51,51,51);
    margin-top: 24px;
    margin-bottom: 24px;
    text-align: center;
  }
  .movelightBar {
    width: 375px;
    position: absolute;
    bottom: 0;
    left: 0;
    transition: left 0.3s linear;
    justify-content: center;
    align-items: center;
  }
  .lightBar {
    width: 56px;
    height: 6px;
    /* position: absolute;
    bottom: 0; */
    /* left: 159.5px; */
  }

  /* ========= 通讯录主体页 ============ */
  .session {
    /* background-color: transparent; */
    position: relative;
    width: 1500;
    flex: 1;
    flex-direction: row;
    align-items: stretch;
    transition: left 0.3s ease-in-out;
  }
  /* 常用联系人 */
  /* -------------------------- */
  /* 侧边搜索栏 */
  .searchAside {
    position: fixed;
    /* right: -16px; */
    right: 0;
    top: 300;
    width: 60px;
    opacity: 1;
    transition: opacity 300 ease-in-out;
  }
  .address {
    width: 750px;
  }
  .searchLetter {
    font-size: 22px;
    height: 30px;
    color: rgb(102,102,102);
    /* padding-top: 1px;
    padding-bottom: 1px; */
    text-align: center;
  }
  .txtLetter {
    position: fixed;
    right: 345px;
    color: #fff;
    background-color: #cccccc;
    width: 60px;
    height: 60px;
    border-radius: 30px;
    font-size: 30px;
    text-align: center;
    padding-top: 10px;
    padding-bottom: 10px;
    opacity: 0;
  }
  /* 联系人明细列表 */
  /* .addressInfo {
  } */
  .addressTitle {
    padding-left: 32px;
    height: 40px;
    justify-content: center;
    background-color: rgb(245,245,249);
  }
  .titleLetter {
    color: rgb(102,102,102);
    font-size: 22px;
  }
  .addressInfoList {
    padding-left: 32px;
    height: 112px;
    flex-direction: row;
    align-items: center;
    border-bottom-width: 1px;
    border-color: rgba(0,0,0,0.08);
    border-style: solid;
  }
  .addressInfoListTeam {
    padding-left: 32px;
    height: 112px;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    border-bottom-width: 1px;
    border-color: rgba(0,0,0,0.08);
    border-style: solid;
  }
  .getMore {
    width: 16px;
    height: 28px;
    margin-right: 32px;
    margin-left:32px;
  }
  .leftSome, .rightSome {
    flex-direction: row;
    align-items: center;
  }
  .avatarImg {
    height: 80px;
    width: 80px;
    border-radius: 40px;
  }
  .avatarTxt {
    justify-content: center;
    height: 80px;
    width: 80px;
    border-radius: 40px;
    /* background-color: rgb(245,245,249); */
    background-color: rgb(0,164,234);
  }
  .avatarName {
    text-align: center;
    color: rgb(255,255,255);
    font-size: 28px;
  }
  .info {
    margin-left: 32px;
    justify-content: center;
  }
  .name {
    font-size: 28px;
    color: rgb(51,51,51);
    /* margin-bottom: 16px; */
  }
  .dept {
    font-size: 24px;
    color: rgb(102,102,102);
    width: 500px;
    lines: 1;
    text-overflow: ellipsis;
  }
  .bgWaterMark {
    position: absolute;
    bottom: 0;
    opacity: .2;
    color:#e9e1e1;
    right: 0;
    left: 0;
  }
  .waterMark {
    flex-direction: row;
    justify-content: space-around;
    min-height: 200px;
    padding-top: 100px;
    /* border-color: blue;
    border-style: solid;
    border-width:1px; */
  }
  .waterMarkText {
    font-size: 26px;
    color: rgb(102,102,102);
    transform:rotate(-36deg);
    /* transform:translateY(100px) rotate(-36deg); */
  }
</style>

<script>
  import fixed from '../../component/fixed.vue'
  import oaHeader from '../../component/oaHeader.vue'
  import { navigatorPushEvent, navigatorPopEvent } from '../../../utils/navigator'
  // import {  } from '../../utils/navigator'
  import { HanZi_PinYin } from '../../../utils/pinyin'
  const storage = weex.requireModule('storage')
  const modal = weex.requireModule('modal')
  const dom = weex.requireModule('dom')
  const animation = weex.requireModule('animation')
  const getOper = new BroadcastChannel('getOper')
  import { picPath } from '../../../utils/picturePath'
  // const navigator = weex.requireModule('navigator')
  // const stream = weex.requireModule('stream')

  export default {
    components: {
      fixed, oaHeader
    },
    data () {
      return {
        cccc: '',
        loadinging:false,
         popIcon: 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABUAAAAkCAYAAABmMXGeAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAAAZdEVYdFNvZnR3YXJlAEFkb2JlIEltYWdlUmVhZHlxyWU8AAADKGlUWHRYTUw6Y29tLmFkb2JlLnhtcAAAAAAAPD94cGFja2V0IGJlZ2luPSLvu78iIGlkPSJXNU0wTXBDZWhpSHpyZVN6TlRjemtjOWQiPz4gPHg6eG1wbWV0YSB4bWxuczp4PSJhZG9iZTpuczptZXRhLyIgeDp4bXB0az0iQWRvYmUgWE1QIENvcmUgNS42LWMwNjcgNzkuMTU3NzQ3LCAyMDE1LzAzLzMwLTIzOjQwOjQyICAgICAgICAiPiA8cmRmOlJERiB4bWxuczpyZGY9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkvMDIvMjItcmRmLXN5bnRheC1ucyMiPiA8cmRmOkRlc2NyaXB0aW9uIHJkZjphYm91dD0iIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtbG5zOnhtcE1NPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvbW0vIiB4bWxuczpzdFJlZj0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL3NUeXBlL1Jlc291cmNlUmVmIyIgeG1wOkNyZWF0b3JUb29sPSJBZG9iZSBQaG90b3Nob3AgQ0MgMjAxNSAoTWFjaW50b3NoKSIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDo1QTExREM0MjREMUIxMUU4OEY4OEY2NDBGOTMyMzY0NiIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDo2QzZEMjY1RTREMUYxMUU4OEY4OEY2NDBGOTMyMzY0NiI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOjVBMTFEQzQwNEQxQjExRTg4Rjg4RjY0MEY5MzIzNjQ2IiBzdFJlZjpkb2N1bWVudElEPSJ4bXAuZGlkOjVBMTFEQzQxNEQxQjExRTg4Rjg4RjY0MEY5MzIzNjQ2Ii8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+uGGeXQAAAmhJREFUSEullsuLjWEcx5+ZE8WCXFZnWLvt3akxFAqFUu7EyZIZMwv/gQwTK52IYcFGUeRSLNxjyYYdZSWXDcrE8fn+nvc53nN9b5/69M7vXXzn6Xmf3+85PbVazTVTrVajv1IzGc/iTjzfqzcFmYq38AhOw8GiobPxEa63ynOjSOgcfIJLrPJcwT15QxfiS5xvlecM7seJPKHLUCvss8ozjENoXz1r6EZ8iDOtcu4PHsBRqyKyhO7FmzjFKud+4Ra8bFWMtKGDOI6TrHLuG67FO1Y1kRTag6fwtFWeT7gan1vVhm6hJbyEx63yvMPl+NaqDnQKDV2yzyrPK1yFH63qQkvv0/czeNxGrShwH7fiT6sSaFgpgaFL4oHXcDOmChT1UALn8dDmL7IXHk2eXfjbqpRYKIGLeTzFuaojTuBRbJ2NCZTK5fI6nvdwur1x7i8exnNW5UArvY762kJdsg0vWpWTdkdqInrmRqG6ArRCob7W+dQIy01vpVLRGVyDX+3N/07SOMtF/fBzAjR4H2B8To5hfU6mpaGjosOv4AX2wqMr4hCm3ut2bTqLh9p0qb3w3MXtmL1NBXv8hccA6uwGNqAmvv5hIu2OlNCKNNWvWuXRyh+jtqgrnUKF+l2jTx8roI/5AuN73kK3UKEN11UyYpUnTLL4njeQFBrQlXIQNReE9la/TLTXLaQNFWoI7XNz9+22KkaWUKGjpqn23Sp/u+pjHrMqImuoeIa6TXWrBvST5yTq9s0VKt7gCnxvlUcfUyOzlDdUfMCV+Noqj34CjRcJFZ+xHzUvAjuKhoofuAkvICfDjf0Djb6D75MHfhwAAAAASUVORK5CYII=',
        //  ismore:false,
        isShow: true, // 显示侧边索引
        // userdata:[],
        userInfoDataList:[],
         searchLetters: ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z'],
         moredata:'',
        avatarImg: 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAE8AAABQCAYAAABYtCjIAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyJpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMy1jMDExIDY2LjE0NTY2MSwgMjAxMi8wMi8wNi0xNDo1NjoyNyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENTNiAoV2luZG93cykiIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6NEFENURDNTU1NjdGMTFFODhGQ0RDMjE5RkQyODU5OTkiIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6NEFENURDNTY1NjdGMTFFODhGQ0RDMjE5RkQyODU5OTkiPiA8eG1wTU06RGVyaXZlZEZyb20gc3RSZWY6aW5zdGFuY2VJRD0ieG1wLmlpZDo0QUE1NzJBODU2N0YxMUU4OEZDREMyMTlGRDI4NTk5OSIgc3RSZWY6ZG9jdW1lbnRJRD0ieG1wLmRpZDo0QUE1NzJBOTU2N0YxMUU4OEZDREMyMTlGRDI4NTk5OSIvPiA8L3JkZjpEZXNjcmlwdGlvbj4gPC9yZGY6UkRGPiA8L3g6eG1wbWV0YT4gPD94cGFja2V0IGVuZD0iciI/PtvtWL0AACqwSURBVHja1H0JsF1l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',
        teamImg: picPath + 'teamImg.png'
      }
    },
    created() {
      storage.getItem('setPersonalMessage', event => {
         var userInfo = JSON.parse(event.data)
         this.cccc = userInfo.persName + userInfo.operNo
      })
    },
    // mounted () {
      // 获取显示字母悬浮圆圈的height,然后计算出悬浮圆圈绝对居中的top高度
      // dom.getComponentRect('viewport', option => {
      //   this.txtLetterTop = (option.size.height - 60)/2 + 'px'
      //   this.searchAsideTop = (option.size.height - this.searchLetters.length * 30)/2 + 'px'
      //   modal.toast({message: this.searchAsideTop})
      //   this.searchAsideOpacity = 1
      // })
      // 获取侧边搜索栏的height,然后计算出搜索栏绝对居中的top高度
      // setTimeout(() => { this.searchAsideOpacity = 1 }, 200)
      // this.getUserInfo()
      // this.getAddressBookData()
      // this.setUserInfoAlot(this.usersInfoList)
    // },
    methods: {
      choose(userInfo) {
         weex.requireModule('commonEvent').getContactById(userInfo.userId, (res1) => {
             var condata = JSON.parse(res1);
             getOper.postMessage(JSON.stringify(condata));
             navigatorPopEvent();
         })
        
      },
      jump () {
        navigatorPopEvent();
        // navigatorPopEvent('addressBook')
      },
      // 通讯录查询
      searchAddressBook (event) {
        this.moredata=event.value
        weex.requireModule('commonEvent').contactSearch2(event.value, (res) => {
          var con = JSON.parse(res)
          // this.userdata=con
          this.userInfoDataList = []
            this.userInfoDataList.push(...con)
          // if(con.toString() !== this.userInfoDataList.toString()) {
          //   this.userInfoDataList = []
          //   this.userInfoDataList.push(...con)
          // } else {
          //   return
          // }
          // this.ismore=true
        })
      },
      more () {
        // modal.toast({message: '成功'})
        this.loadinging=true
        weex.requireModule('commonEvent').searchMore(this.moredata, (res) => {
        var con = JSON.parse(res)
          this.loadinging=false
          // this.userdata=this.userdata.push(con)
          this.userInfoDataList.push(...con)
        })
      },
      // 点击侧边栏搜索字母滚动到指定位置
      scrollTo (ref) {
        var el = this.$refs[ref][0]
        dom.scrollToElement(el, { offset: this.Env.deviceModel === 'iPhone10,3' ?  '-68' : this.Env.platform === 'iOS' ? '-40' : '-50' })
        // 监听悬浮圆圈现实的字母
      },
      getAddressBookData () {
        weex.requireModule('commonEvent').getAllContact('weex', (res) => {
          var con = JSON.parse(res)
          // if(con.list.length!=0){
          //   // this.ismore=true
          // }
          // else{
          //   // this.ismore=false
          // }
          // modal.toast({ message: res })
          // this.usersInfoList.push(...con.list)
          this.setUserInfoAlot(con.list)
        })
      }
      // 后台返回数据处理按字母排序
    }
  }
</script>

