# Tool
基于Jquery.js日常应用到的工具,主要用于DOM操作和数据处理，如表单数据转化成JSON，设置初始日期，正则验证，JSON数组转换，JSON数组分类等



### 用法

```
script标签导入

<script type="text/javascript" src="js/jquery-3.1.1.min.js"></script>
<script type="text/javascript" src="js/Tool.js"></script>

npm 安装

npm install --save-dev jquery
npm install --save-dev jqtools

var $=require('jqtools');//$仍然是JQ对象，此时的JQ对象已经做了扩展操作，函数使用方法如下。
最后使用webpack打包生成即可。


如果需要使用弹窗插件，请安装sweetalert

npm install --save-dev sweetalert
require('sweetalert');

安装包中已经存在sweetalert必要的样式文件，只需要引入便可直接使用。
使用方法请参考 sweetalert的官网，用法一致

```

### 分类
- 一类是扩展空间名tools,此空间名下的方法调用为$.tools.xxx()
- 另一类则是直接扩展fn空间名的属性和方法 $.fn.xxx()或者$('x').xxx()

## 将表单数据转换为JSON -***formTojson***
*formTojson(newObj, join)*
- newObj 允许在表单基础上添加其他数据
- join 新添加的属性与表单中的属性重合时是否合并到已有属性并用逗号隔开，true合并，false替换form表单属性值
- 返回值：JSON数据
```html
<form>
	<input type="text" name="name" value="siyuan" />
	<input type="text" name="password" value="123" />
</form>
<!--<input type="text" id='email' name="email"  value="xxx@xxx.com" />-->
```

```javascript
//也可以选择$('form,#email').formTojson();这样来调用
$('form').formTojson({email:'xxxx@163.com'},true);//{"name":"siyuan","password":"123","email":"xxxx@163.com"}
$('form').formTojson({name:'xxxx@163.com'},true);//{"name":"siyuan,xxxx@163.com","password":"123"}
$('form').formTojson({name:'xxxx@163.com'},false);//{"name":"xxxx@163.com","password":"123"}
```

## 初始化日期控件的值为当天日期 -***setCurDate***
*将jq选择器选择的日期控件默认为当前日期*

```html
<input type="date" name="date" id="date" value="" />
<input type="date" name="date1" class="date" value="" />
```

```

$('#date,.date').setCurDate();
```

## 检测日期选择条件是否正确 -***checkDate***
*日期选择条件的检查，结束日期必须大于等于初始日期*
- 返回值true/false
```html
<input type="date" name="date" id="date" value="" />
<input type="date" name="date1" class="date" value="" />
```

```javascript
//第一个选取的对象是初始日期，第二个则为结束日期，返回boolean
$('#date,.date').checkDate();//true/false
```

## 获取当前具体日期时间 -***getCurDate***
*getCurDate(split, start, end)*
- split 年月日的分隔符
- start 年月日时分秒，对应0-5 ，从哪个单位开始
- end 年月日时分秒，对应0-5 ，到哪个单位结束
- 返回值：日期字符串（日期选择条件的检查，结束日期必须大于等于初始日期）


```javascript
//第一个选取的对象是初始日期，第二个则为结束日期，返回字符串
//参数getCurDate(split, start, end),split为日期分隔符,start,end参数为0-5，分别对应年月日时分秒
$.tools.getCurDate('-',1,4);//"10-31 17:49"
```

## 指定当前元素与下一个元素的关系 -***nextElement***
*nextElement(callback[t,i])*
- callback 回调函数，指定选择的所有元素与其下一个元素之间的动作联系
- t 选取的所有元素
- i 当前元素在t元素列表中的索引
在JQ对象中，指定选取对象的当前元素与下一个元素的关系，比如，按下enter键，下一个元素要做什么动作

```html
<input type="text" name="c" id="c" value="5.254" /> 
<input type="text" name="d" id="d" value="4" />

```

```javascript
//nextElement(callback[t,i])=>t表示选取的所有元素，$(t[i])表示每一个元素,$(t[i+1])则为下一个元素
//以下内容的效果为，绑定所有input元素，enter键keyup的时候，使下一个元素onfocus
$('input').nextElement(function(t, i) {
				$(t[i]).on('keyup', function(event) {
					if(event.keyCode == 13) {
						$(t[i + 1]).focus();
					}
				})
			});

```

## 深拷贝数组或者JSON对象 -***deepCopy***
**deepCopy(obj)**
- obj 复制目标，数组或JSON
- 返回新对象
```javascript
	var arr=[1,2,3];
	var jsons={a:1,b:2,c:3};
	var newArr=$.tools.deepCopy(arr);
	var newJson=$.tools.deepCopy(jsons);
	arr[0]=9;
	jsons.a=9;
	console.log(JSON.stringify(arr));//[9,2,3]
	console.log(JSON.stringify(jsons));//{"a":9,"b":2,"c":3}
	console.log(JSON.stringify(newArr));//[1,2,3]
	console.log(JSON.stringify(newJson));//{"a":1,"b":2,"c":3}
```

## 检验小数位数 -***RegNumber***
**RegNumber(num)**
- num 检验小数点后的位数
- 返回值 true/false
```html
	<input type="text" name="c" id="c" value="5.294" /> 
			
```

```javascript
//RegNumber(num) num=>检测小数点位数，该值默认为两位小数，不可小于0
$('#c').RegNumber(3);//true
```

## 检验手机位数 -***RegPhone***
**RegPhone()**
- 只检验是否11位，不检测有效性
- 返回值 true/false
```javascript
//只单纯检测是否为11位，不作其他有效值判断，如有需要自行改变正则匹配
console.log($('#c').RegPhone());
```

## 检验JQ对象中是否含有空值 - ***hasEmpty***

```html
<input type="text" name="c" id="c" value="1234567w8901" /> 
			<input type="text" name="d" id="d" value="" />

```

```
$('#c,#d').hasEmpty();//true
```


## 将数据填入表单数据中 - ***dataToInput***
**dataToInput(obj)**
- obj 数组或JSON
- 根据传入对象数据顺序填入表单控件中,超过输入控件的数据不会进行处理

```javascript
var a=[1,2];
var jsons={a:'a',b:'b'};
$('#c,#d').dataToInput(a);
$('#c,#d').dataToInput(jsons);
```

## jsonp跨域请求 - ***jsonp***
**jsonp(url, callbackFunc, data);** 
- url 请求地址
- callbackFunc  回调函数名
- data 传输的数据
```javascript
$.tools.jsonp('http://www.xxx.com/xx.php','getResponce',{name:'siyuan'});
//需要写一个与getResponce一样的回调函数。此处后台接受请求返回的格式如下
//事实上 jsonp也并非一定要通过回调函数，只要跟后台返回的格式一一对应，我们同样能用数组、对象来存储数据
/*
  $fun=$_GET['callback'];//获取传递过来的函数名
$name=$_GET['name'];
 $data="{age:18,name:'$name'}";//需要返回的数据
 echo "$fun($data)";
 */

function getResponce(res)
{
	alert(res.name+res.age);//siyuan18
	
}
```
## 获取随机数 - ***getRandom***
**getRandom(num, type)**
- num 随机数的位数
- type 随机数的类型，'number','word','hybrid'

``` 
getRandom(num, type);//num:获取位数，type:'number','word','hybrid'，意思是返回纯数字，纯字母，或者混合型随机数
console.log($.tools.getRandom(5,'number'));//00622
console.log($.tools.getRandom(5,'word'));//tbEYI
console.log($.tools.getRandom(5,'hybrid'));//F9liT

```


## 倒计时 -***countDown***
```javascript
//$.tools.countDown(time,Func[cd]);
//time:倒计时，cd:剩余时间,返回值：一个可以传递给 Window.clearInterval() 从而取消对 code 的周期性执行的值。
$.tools.countDown(60,function(cd){	
	console.log(cd);	
});
```

## JSON数组分组函数 - ***groupBy***
**groupBy(jsonArray, groupby, res)** 
- jsonArray:json数组
- groupby:按某个属性分组 
- res：按groupby分组后得到的集合属性
- 【返回值】分组后的JSON数组

```
var arr=[{age:15,name:'张三'},{age:18,name:'李四'},{age:12,name:'王五'},{age:15,name:'小王'},{age:12,name:'老王'}];
var arr1=[{age:15,name:'张三'},{age:18,name:'李四'},{age:12,name:'张三'},{age:15,name:'小王'},{age:12,name:'张三'}];

console.log(JSON.stringify($.tools.groupBy(arr,'age','name')));//"[{"age":15,"name":"张三,小王"},{"age":18,"name":"李四"},{"age":12,"name":"王五,老王"}]"
console.log(JSON.stringify($.tools.groupBy(arr1,'name','age')));//"[{"name":"张三","age":"15,12,12"},{"name":"李四","age":18},{"name":"小王","age":15}]"

```

## JSON数组转换成数组- ***jsonArrayToArray***
		 * 例子：var n = jsonToarray(list, 'pay', 'processType');//如果只传入数组，则默认保存所有JSON数据
		 * 输出 "[["365","2"],["3265","22"]]" ，结果顺序为[[pay,processType],[pay,processType]];
		 * 
		 * @param {Object} js JSON数组
		 * @param {Object} arg1 指定需要的JSON数据，也可以用来指定数组排序
		 *@param {Object} arg2
		 *@param {Object} arg3 ....
```javascript
var arr=[{age:15,name:'张三'},{age:18,name:'李四'},{age:12,name:'王五'},{age:15,name:'小王'},{age:12,name:'老王'}];
console.log(JSON.stringify($.tools.jsonArrayToArray(arr,'age','name')));// "[[15,"张三"],[18,"李四"],[12,"王五"],[15,"小王"],[12,"老王"]]"
console.log(JSON.stringify($.tools.jsonArrayToArray(arr,'age')));//[[15],[18],[12],[15],[12]]
console.log(JSON.stringify($.tools.jsonArrayToArray(arr)));//"[[15,"张三"],[18,"李四"],[12,"王五"],[15,"小王"],[12,"老王"]]"

```
## 合并数组- ***mergeArray***
**mergeArray(first,secone,newArray)**
- first:合并的第一个数组
- secone：合并的第二个数组
- newArray是否需要返回新数组，还是合并到first数组，默认(false)合并到first数组并返回,true则返回新数组

```
var a=[1,2],b=[2,3],c=[4,5];
var t=$.tools.mergeArray(a,b,true);
var t=$.tools.mergeArray(a,b,false);
var t=$.tools.mergeArray(a,b);
```

## 搜索数组内容- ***arrayHasElement***
**arrayHasElement(obj,arr);**
- obj:需要搜索的对象，目前只支持array/json   
- arr:搜索目标，查找arr中是否有obj中的数据
- 返回查找到的数据数组
```
$.tools.arrayHasElement([1,2,'ds'],[1,2,'a','v','d']);//[1,2]
```

## 对象赋值- ***jsonAssignment***
**jsonAssignment(objA,objB)**
- objA 被赋值的对象
- objB 赋值的对象
```javascript
$.tools.jsonAssignment(a,b);
//将b的属性全部赋值给a，b的属性必须与a完全一致才允许赋值，成功返回新的JSON, 失败返回false
```

## json数组属性值替换- ***jsonArrayReplace***
**jsonArrayReplace(obj, prop, oldV, newV);**
- obj 需要被替换属性的json数组
- prop 被替换的属性
- oldV 被替换的旧值
- newV 新值 
```
var js=[{a:1},{a:2},{a:3}];	
$.tools.jsonArrayReplace(js,'a','1','*');
console.log(JSON.stringify(js));//[{"a":"*"},{"a":2},{"a":3}]
```

## 索引查找- ***findIndex***
*findIndex(obj,val,condition);*
- obj 数组，JSON，JSON数组(第三个参数指定属性)
- val 需要查找的值
- condition 如果obj是JSON数组，则需要指定查找的属性


```javascript
/*$.tools.findIndex(arr, val);
arr可以是数组也可以是json
val要查找的值

返回值：
没有找到 返回 null
只有一个直接返回索引
多个结果返回 结果集(数组)
*/
var arr={a:'1',b:'2',c:'8',d:'2'};
$.tools.findIndex(arr,2);//['b','d'];
$.tools.findIndex(arr,28);//null
$.tools.findIndex(arr,8);//c

var arrAll=	[{name:'a',age:1,l:5},{name:'b',age:2,l:5},{name:'c',age:3,l:5},{name:'d',age:4,l:5},{name:'c',age:3,l:5}];
var arr=[1,2,3,4,1];
$.tools.findIndex(arr,'1');//[0,4]
$.tools.findIndex(arrAll,'3','age');//[2,4]

```

## 查找字符串中的某一段- ***findString***
**findString(str,val)**
- str 查找的目标
- val 查找的字符串片段 
```
$.tools.findString(str,val);
$.tools.findString("siyuan","isy");//false
$.tools.findString("siyuan","sy");//true

```


## 数据队列- ***arrayQueue***
**arrayQueue(arr,turn)**
- arr 循环排列的数组
- turn 循环排列的方向，默认false 向后排（第一到最后），true向前排(最后到第一)
```javascript
/**
	 * JSON数据‘轮流排队’,用于一组循环排列的数据 [1,2,3,4]=》[2,3，4,1] 
	 //如果是 JSON数组，需要排序某些属性而不更改原本的位置，则单独抽取出那个属性作为数组，再重新赋值
	 //结合$.tools.arrayInputJson函数使用
	 * @param {Object} arr 需要排列的数组
	 * @param {Object} turn 第一个排到最后面 OR  最后面排到最前面 默认向后排队(false)
	 */
	var arr=[1,2,3,4];
	$.tools.arrayQueue(arr,true);//[4,1,2,3]
	$.tools.arrayQueue(arr);//默认值为false，向后排 [2,3,4,1]

```

## 身份证验证- ***RegId***
```javascript
$('x').RegId();//true or false

```


## JSON数组属性抽取- ***extractArrayFromJson***
**extractArrayFromJsonArray(obj,par1,par2....);**
- obj json数组
- par1 提取的属性1 
- par2 提取的属性2
- par3 ...
- 返回值 如果只提取一个属性，则返回数组，若提取多个属性，则返回JSON数组
```javascript

var arrAll=	[{name:'a',age:1},{name:'b',age:2},{name:'c',age:3},{name:'d',age:4}];
$.tools.extractArrayFromJson(arrAll,'name');//"["a","b","c","d"]"
$.tools.extractArrayFromJson(arrAll,'name','age');//[{name:'a',age:1},{name:'b',age:2},{name:'c',age:3},{name:'d',age:4}]

```

## JSON数组属性赋值 - ***arrayInputJsonArray***

```javascript
var arrAll=	[{name:'a',age:1},{name:'b',age:2},{name:'c',age:3},{name:'d',age:4}];
var arr=[4,5,6,7];
$.tools.arrayInputJson(arrAll,arr,'age');//[{name:'a',age:4},{name:'b',age:5},{name:'c',age:6},{name:'d',age:7}]
```

## 查找数组或JSON数组索引 - ***findIndexWithCallback***
**findIndexWithCallback(obj,callback[curVal,index]);**
- obj 索引对象
- callback 条件回调函数, curVal 为当前的元素， index当前元素索引 ,this 为索引结果集（数组）
```javascript

var t=$.tools.findIndexWithCallback([{a:1},{a:2}],function(curVal,index){
				curVal.a>1&&this.push(index);			
			});
			
var t1=$.tools.findIndexWithCallback([1,2,3,4,5,6],function(curVal,index){
				curVal>3&&this.push(index);			
			});

//t=> [1]
//t1 => [3,4,5]
```

## 拼接JSON数据 字符串
**jsonStringJoin(obj,arr,sep)** 
- obj JSON对象
- arr 数组  需要拼接的属性名 
- sep 分隔符
```javascript
var t={name:'siyuan',addr:'China-sichuan',phone:'13xxxxx',age:'18'};
$.tools.jsonStringJoin(t,['name','age','phone','addr'],'@');//siyuan@18@13xxxxx@China-sichuan
```

## 按ASCII编码 从小到大排序 数组
**AsciiSort(obj,attr)，如果排序的值为非number类型，按照unicode排序，也相当于ASCII排序【升序】，如果是number则按大小排序**
- obj 数组
- attr 如果obj是JSON数组，则可以指定按某个属性的值排序
```javascript

var newarr=AsciiSort([{a:100,b:5,f:85,c:2},{a:98},{a:155}],'a');//[{"a":98},{"a":100,"b":5,"f":85,"c":2},{"a":155}]
var carr=AsciiSort([{a:'mch_id',b:5,f:85,c:2},{a:'openid'},{a:'appid'}],'a');//[{"a":"appid"},{"a":"mch_id","b":5,"f":85,"c":2},{"a":"openid"}]
var arr=AsciiSort([100,98,155]);//98,100,155
var c=AsciiSort(['mch_id','body','openid']);//body,mch_id,openid

//=======
var str="body=物流/快递公司&mch_id=1497772112&appid=xxx&notify_url=xxx&nonce_str=OUz1Hvfs2hixxzcnreTzrBScLw6R8l3F&openid=xxx-c8EzEA&out_trade_no=000000081517359702559&spbill_create_ip=58x.139&total_fee=1&trade_type=JSAPI"
var arr=str.split('&');
console.log(AsciiSort(arr));//appid=xxx,body=物流/快递公司,mch_id=1497772112,nonce_str=OUz1Hvfs2hixxzcnreTzrBScLw6R8l3F,notify_url=xxx,openid=xxx-c8EzEA,out_trade_no=000000081517359702559,spbill_create_ip=58x.139,total_fee=1,trade_type=JSAPI


```


## 轮播图片
**banner(id,width,speed)**
- id 轮播组件的id
- width 轮播图片的宽度
- speed 轮播的速度
```html
<style type="text/css">
			#header
			{
				position: relative;
				width: 600px;
				height: 200px;
				overflow: hidden;
			}
			#header img
			{
				position: absolute;
				
			}
		</style>
		<div id="header">
			<img src="img/2.png" />
			<img src="img/3.png" />
			<img src="img/4.png" />
			<img src="img/404.jpg" />
			<img src="img/bg.jpg" />			
		</div>

<script>
$.ui.banner('header',600,3000);//600与样式中的宽度保持一致，speed为播放速度

</script>

```

## 新增圆形菜单
**circularMenu(id,radius,sp)**
- id 需符合jq选择器，比如id,class等等'#center'，表示主菜单
- radius 主菜单圆心与子菜单的圆心距
- sp 起始位置，'left' 'up' 'right' 'down'
- **样式中，主菜单和子菜单的大小可以改变，整个菜单的位置是根据.center来移动的，所以可以调节left和top**
```html
<style type="text/css">
#menu {
				width: 200px;
				height: 200px;
				background: red;
				position: relative;
			}
			
			#menu .sub {
				width: 40px;
				height: 40px;
				background: lightskyblue;
				position: absolute;
				border-radius: 50%;
				opacity: 0;
			}
			
			#menu .center {
				width: 50px;
				height: 50px;
				border-radius: 50%;
				background: lightskyblue;
				position: absolute;
				left: 100px;
				top: 100px;
			}
			
		</style>	
			<div id="menu">
			<div class="center" id="center"></div>
			<div class="sub" id="menu1"></div>
			<div class="sub" id="menu2"></div>
			<div class="sub" id="menu3"></div>
			<div class="sub" id="menu4"></div>

		</div>

```

```javascript
$.ui.circularMenu('#menu .center',100,'left');
```


## 新增等待UI
**showWaiting:(id, number, spacing, radius)**
- id canvas id
- number 点的个数
- spacing 点间距
- radius 半径

```javascript
$.ui.showWaiting('canvas',3,20,5);
```


## 新增金额划分，如5000 =>5,000
**money(m)**
- m 数字金额
```
$.tools.money(50032154654165400)//50,032,154,654,165,400
```

**_checkOption(opt,defaultOption)**
- opt 传入object
- defaultOption 自定义必须属性
```javascript
defaultOption={a:'',b:'',c:''}
opt={a:'1',b:'2'}
$.tools._checkPotion(opt,defaultOption);// false 提示传入参数中缺少【c】属性
```

## 更新日志

*2018-01-06* 
   - **修改extractArrayFromJson函数更名为extractArrayFromJsonArray，允许从json数组中提取一个或多个属性值并返回数组对象**
   - **修改findIndex函数，允许查找json数组**
   - **arrayInputJson更名为arrayInputJsonArray**
   - **修改nextElement函数问题,用法有修改，请参考函数说明**
   - **规范文档用语： 数组，JSON，JSON数组对应函数名中的  array,  json  jsonArray=》[1,2,3] ,{a:1,b:2},  [{a:1},{b:2}]**
   - **修复extractArrayFromJsonArray获取属性一直获取的是最后一个匹配值的问题**
   
*2018-01-11*
- **提供sweealert弹窗必须文件，可以安装npm install sweetalert 并在代码中引用即可**

*2018-01-15*
- **新增findIndexWithCallback，通过回调函数查找索引，支持数组和JSON数组**

*2018-01-16*
- **新增jsonStringJoin，拼接JSON数据**

*2018-01-24*
- **新增ui命名空间，$.ui,新增轮播图片$.ui.banner(id,width,speed)**

*2018-01-31*
- **合并formTojson和dataTojson两个函数为formTojson，选取元素不仅限于form表单，而是所有具有name属性的input元素，比如$('form,#a,#b').formTojson({})**

*2018-02-01*
- **新增数组按照ASCII排序 AsciiSort**
- **修改AsciiSort，排序规则，修复排序出错的问题**

*2018-02-02*
- **新增UI作用域，circularMenu，四分之一圆形菜单**

*2018-02-06*
- **新增简单等待UI,canvas**
*2018-04-19*
- **新增金额显示**
*2018-05-30*
- **新增_checkOption函数，检测object参数的必需性**