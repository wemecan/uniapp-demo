<template>
	<view class="body" >
        
        <!-- 自定义导航 -->
        <!-- #ifndef APP-PLUS || H5 -->
        <view style="display: flex;
        		align-items: center;
        		padding:0 20upx;height: 88upx;
        		position: fixed;z-index: 9999;
        		left: 0;
        		top: 0;
        		right: 0;
        		background: #FFFFFF;">
        			<view class="iconfont icon-sousuo" 
        			style="position: absolute;left: 30upx;color: #CCCCCC;"></view>
        			<input style="flex: 1;padding: 5upx 0 5upx 50upx;border-radius: 4px;background: #F7F7F7;" 
        			placeholder="搜索用户" disabled=true @tap="addFriend"
        			placeholder-style="color: #CCCCCC;"/>
        			
        		</view>
        		<view style="height: 88upx;"></view>
        <!-- #endif -->
       
        
        <!-- 操作菜单 -->
       <paper-left-popup
       :isPopupShow="isPopupShow" @hidePopup="hidePopup" 
       @addFriend="addFriend" @clearPaperList="clearPaperList">
       </paper-left-popup>
        
        <view class="paper-friendlist u-f-ajc" @tap="NavToFriendList">
            我的好友
            <view class="icon iconfont icon-jinru"> </view>
        </view>
        <!-- 消息 列表-->
         <block v-for="(item,index) in list" :key="index">
            <paper-list :item="item" :index="index"></paper-list>
        </block>
        
        
        <!-- 上拉加载 -->
        <load-more :loadText="loadText"></load-more>
	</view>
</template>

<script>
    import paperList from '../../components/paper/paper-list.vue';
    import loadMore from "../../components/common/load-more.vue";
    import paperLeftPopup from '../../components/paper/paper-left-popup.vue';
	export default {
        components:{
            paperList,
            loadMore,
            paperLeftPopup
        },
		data() {
			return {
                isPopupShow:false,//默认隐藏
                loadText:"上拉加载更多",
				list:[
                    {
                        userPic:"",
                        userName:"此功能仅为前端展示用",
                        time:"12:45",
                        msg:"OPPS",
                        unreadNum:0
                    },
                    {
                        userPic:"../../static/demo/userpic/14.jpg",
                        userName:"ATM",
                        time:"12:45",
                        msg:"[图片]",
                        unreadNum:0
                    },
                   
                    {
                        userPic:"../../static/demo/userpic/14.jpg",
                        userName:"ATM",
                        time:"12:45",
                        msg:"[图片]",
                        unreadNum:6
                    },
                    {
                        userPic:"../../static/demo/userpic/14.jpg",
                        userName:"ATM",
                        time:"12:45",
                        msg:"[图片]",
                        unreadNum:0
                    },
                ]
			}
		},
        onPullDownRefresh() {
            this.getData();
        },
        onReachBottom() {
            this.loadMore();
        },
        onNavigationBarButtonTap(e) {
            if(e.index == 1){
                this.isPopupShow = true;
            }
            if(e.index == 0){
                this.User.navigate({
                   url:"../user-list/user-list"
               });
            }
        },
		methods: {
            showPopup(){
                 this.isPopupShow = true;
            },
            hidePopup(){
                 this.isPopupShow = false;
            },
            //操作菜单
            addFriend(){
                console.log("添加好友");
                
                this.hidePopup();
                uni.navigateTo({
                    url:'../search/search?searchType=user'
                })
            },
            //清空列表
            clearPaperList(){
               console.log("清空缓存");
               var subList =  this.list;
               for(var i=0;i<subList.length;i++){
                   subList[i].unreadNum = 0;
               }
               this.list = subList;
               subList = null;
               this.hidePopup();
            },
            //上拉加载
            loadMore(){               
                if(this.loadText != "上拉加载更多"){
                    return;//如果正在加载中(＾o＾)ﾉ或没有数据可以加载，则停止请求
                }
                console.log("running1")
                this.loadText = "加载中(＾o＾)ﾉ";
                //修改状态
                setTimeout(()=> {                  
                    //示例:加载2000ms后从服务端获取了新的数据
                    let obj = 
                    {
                            userPic:"../../static/demo/userpic/14.jpg",
                            userName:"ATM",
                            time:"12:45",
                            msg:"more[图片]",
                            unreadNum:0                      
                    };
                    this.list.push(obj);//追加                   
                    this.loadText = "上拉加载更多";     //复原状态              
                }, 2000);
                
                //loadText = "没有更多数据";           
            },
			// 获取数据
            getData(){
                setTimeout(()=> {
                let arr = [
                    {
                        userPic:"../../static/demo/userpic/14.jpg",
                        userName:"ATM",
                        time:"12:45",
                        msg:"ref[图片]",
                        unreadNum:6
                    },
                    {
                        userPic:"../../static/demo/userpic/14.jpg",
                        userName:"ATM",
                        time:"12:45",
                        msg:"ref[图片]",
                        unreadNum:0
                    },
                ];
                this.list = arr;
                uni.stopPullDownRefresh();
                //获取数据
                //赋值
                //关闭下拉刷新
                }, 2000);
            },
            NavToFriendList(){
                uni.navigateTo({
                    url:"../user-list/user-list"
                })
            },
          
		}
	}
</script>

<style>
    .body{
        padding: 0 20upx;
    }
   .paper-friendlist{
       padding:30upx 0;
   }
     
</style>
