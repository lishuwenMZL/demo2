<template>
<div>
  <header class="aui-bar aui-bar-nav aui-bar-light">
   <div class="aui-searchbar" id="search">
    <div class="aui-searchbar-input aui-border-radius" tapmode>
        <i class="aui-iconfont aui-icon-search" @touchend="doSearch()"></i>
        <form action="javascript:search();">
            <input @input="handleFocus" type="search" ref="search" placeholder="搜索商家名字" id="search-input" autofocus v-model="value">
            <i class="aui-iconfont aui-icon-close inputDelete"  v-show="show" @touchend="cancle()"></i>
        </form>
    </div>
    <div class="aui-searchbar-cancel" tapmod><a href="/#/index">取消</a></div>
   </div>
  </header>
  <section>
    <div class="history">
      <p class="title" @touchend="searchHistory()" v-show="historyShow">搜索历史</p>
      <ul class="list">
        <li v-for="(item,index) in listArr" :key="index">
          <i class="aui-iconfont aui-icon-date icon"></i>
          <span class="item">{{item}}</span>
          <i class="aui-iconfont aui-icon-close icon" @touchend="deleted(index)"></i>
        </li>
      </ul>
      <div class="clear" @touchend="clearHistory()" v-show="historyShow">清空历史记录</div>
    </div>
  </section>
</div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      value:'',
      listArr:[],
      localArr:[],
      show:false,
      historyShow:false
    }
  },
  methods:{
    handleFocus(){
      this.show=true; // 监听输入事件
    },
    doSearch(){ //添加历史记录
       if(this.value){
          this.show=true; 
          let pos=this.localArr.indexOf(this.value);//如已经搜索过的记录不再重复出现在列表中
          if(pos<0){
            this.localArr.unshift(this.value);//value分别添加到页面跟本地的数组中
            this.listArr.unshift(this.value); 
            localStorage.setItem("localArr",JSON.stringify(this.localArr));//把数组更新到本地上 
          }else{
            return;
          } 
          this.historyShow=true;      
       }
    },
    cancle(){ //取消
      this.value='';
    },
    deleted(index){//删除某个历史记录
      this.listArr.splice(index,1);
      this.localArr.splice(index,1);
      localStorage.setItem("localArr",JSON.stringify(this.localArr));//把数组更新到本地上 
    },
    clearHistory(){ // 清空历史记录
        localStorage.clear();
        this.listArr='';
        location.reload();
    },
    searchHistory(){// 搜索历史
      this.$refs.search.focus();
    }
  },
  mounted () {
    afterRouters['/find']=true;
    var obj=JSON.parse(localStorage.getItem("localArr")); //初始化页面时先把本地的数据加载到页面
    if(obj){
      this.localArr=obj; //保存本地数据的数组
      for(var i=0;i<obj.length;i++){
        this.listArr.push(obj[i]);// 保存页面数据的数组
      }
    }
  }
}
</script>
<style scoped>
  .aui-bar-light{
    background:#fd6a27;
    color:#000;
    width:100%;
    height:2rem;
  }
  .aui-bar-nav.aui-bar-light .aui-iconfont{
    color:#666;
  }
  .aui-searchbar{
    width:98%;
    background:none; 
  }
  .aui-searchbar-input{
    border-radius: 0.3rem;
    border:0.01rem solid #fff;
  }
  .aui-searchbar .aui-searchbar-cancel{
    color:#fff;
    z-index:999;
    margin-right: 0;
  }
  input:-moz-placeholder,textarea:-moz-placeholder {   
    color: #CCCCCC;   
  }    
  input:-ms-input-placeholder,textarea:-ms-input-placeholder {   
    color:#CCCCCC;   
  } 
  input::-webkit-input-placeholder,textarea::-webkit-input-placeholder {   
    color: #CCCCCC;   
  }  
  /* 输入框中的 ×  */
  .inputDelete{
    width:1rem;
    height:1rem;
    line-height: 1rem;
    display: inline-block;
    color:#000;
    position: relative;
    top:-1.5rem;
    left:6rem;
    font-size:0.6rem;
    background: #ccc;
    border-radius: 0.5rem;
  }
  .history{
    text-align:left;  
    margin:0.7rem;
    font-size:0.7rem;
  }
  .history ul li{
    width:100%;
    height:2rem;
    line-height:2rem;
    border-bottom:1px solid #D6D5D5;
  }
  .history .clear{
    margin-top:0.5rem;
    text-align:center;
  }
  .history .list .item{
    width:87%;
    display: inline-block;
    padding-left:0.5rem;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
  .history .list .icon{
    position: relative;
    top:-0.7rem;
  }
</style>
