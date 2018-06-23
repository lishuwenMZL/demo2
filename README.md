
VUE开发笔记--滚动加载与滚动跳转

1、滚动加载的原理：
判断滚动条到底部，需要用到DOM的三个属性值，即scrollTop、clientHeight、scrollHeight。
scrollTop为滚动条在Y轴上的滚动距离。
clientHeight为内容可视区域的高度。
scrollHeight为内容可视区域的高度加上溢出（滚动）的距离。
从这个三个属性的介绍就可以看出来，滚动条到底部的条件即为scrollTop + clientHeight == scrollHeight。（兼容不同的浏览器）。

个人小demo：
            var that=this;
            
            $(window).scroll(function(){
            
                let scrollTop=$(this).scrollTop()
                
                let scrollHeight=$(document).height()
                
                let windowHeight=$(this).height()
                
                if(scrollTop+windowHeight===scrollHeight){
                
                    $.ajax({     
                    
                        type:'POST',  
                        
                        url:http,  
                        
                        dataType:'json',
                        
                        data:{//},
                        
                        success:(data)=>{   
                        //
                        },
                        
                        error:function(jqXHR){
                        
                            console.log("请求失败");
                            
                        }
                        
                    });
                    
                } 

2、导航栏滚动跳转的实现原理与API

document.documentElement.scrollTop与document.body.scrollTop实现的效果是一样的，只是为了兼容不同的浏览器进行选择，浏览器识别了

document.documentElement.scrollTop时，document.body.scrollTop则会为0，反之也是。详细介绍见下转载

https://www.cnblogs.com/boyuguoblog/p/7744747.html 

小demo：

    toggle(index){
    
      let top=this.$refs.infor[index].offsetTop;//距离顶部的距离
      
      document.documentElement.scrollTop=top-50;//滚动距离
      
      document.body.scrollTop=top-50;  //兼容性 
      
    },
