```javascript

    <button>运动到400然后回来</button>
    <div></div>


    <script>

        var btnArr = document.getElementsByTagName("button");
        var div = document.getElementsByTagName("div")[0];

        btnArr[0].onclick = function () {
            var json1 = {"left":300,"top":200,"width":300,"height":200,"opacity": 30,"zIndex": 1};
            animate(div,json1);
        }



        //参数变为3个
        function animate(ele,json,fn){
            //先清定时器
            clearInterval(ele.timer);
            ele.timer = setInterval(function () {
                //开闭原则
                var bool = true;


                //遍历属性和值，分别单独处理json
                //attr == k(键)    target == json[k](值)
                for(var k in json){
                    //四部
                    var leader;
                    //判断如果属性为opacity的时候特殊获取值
                    if(k === "opacity"){
                        leader = getStyle(ele,k)*100 || 1;
                    }else{
                        leader = parseInt(getStyle(ele,k)) || 0;
                    }

                    //1.获取步长
                    var step = (json[k] - leader)/10;
                    //2.二次加工步长
                    step = step>0?Math.ceil(step):Math.floor(step);
                    leader = leader + step;
                    //3.赋值
                    //特殊情况特殊赋值
                    if(k === "opacity"){
                        ele.style[k] = leader/100;
                        //兼容IE678
                        ele.style.filter = "alpha(opacity="+leader+")";
                    //如果是层级，一次行赋值成功，不需要缓动赋值
                        //为什么？需求！
                    }else if(k === "zIndex"){
                        ele.style.zIndex = json[k];
                    }else{
                        ele.style[k] = leader + "px";
                    }
                    //4.清除定时器
                    //判断: 目标值和当前值的差大于步长，就不能跳出循环
                    //不考虑小数的情况：目标位置和当前位置不相等，就不能清除清除定时器。
                    if(json[k] !== leader){
                        bool = false;
                    }
                }

                console.log(1);
                //只有所有的属性都到了指定位置，bool值才不会变成false；
                if(bool){
                    clearInterval(ele.timer);
                    //所有程序执行完毕了，现在可以执行回调函数了
                    //只有传递了回调函数，才能执行
                    if(fn){
                        fn();
                    }
                }
            },25);
        }




        //兼容方法获取元素样式
        function getStyle(ele,attr){
            if(window.getComputedStyle){
                return window.getComputedStyle(ele,null)[attr];
            }
            return ele.currentStyle[attr];
        }


    </script>

```