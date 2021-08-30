1.test3 方式

https://services.arcgis.com/V6ZHFr6zdgNZuVG0/arcgis/rest/services/weather_stations_010417/FeatureServer/0

1,先弄双层显示



寻找layer click



legent 滑条组件 渐变条

https://developers.arcgis.com/javascript/latest/sample-code/sandbox/index.html?sample=views-composite-views



https://developers.arcgis.com/javascript/latest/sample-code/sandbox/index.html?sample=visualization-vv-opacity

比较好的透明度示例



uber 开源的库 derk



arcgis runtime  是一种 cs程序  bs

华为单机系统,cs程序, 销售带电脑抛,  cs c++ 接口,qt也行,  .net最快, js没办法读本地资源, runtime最便宜,  触摸屏, 一个,看您的

功能, 基于现在都是java .net很少,  runtime是长期展示, 单机能不能解决? 

还有一个问题,就是说,请求摄影厂商slpk 导出格式就ok

smard 3d 也可以,提供了西安交大的数据, 目前没有香港数据, 精模格式数据都是可以的, 用arcgis 都是, 地形dem数据, 网上都有30

米的, 建议不要碰5m一下数据, 1m绝密数据,性能展示上性能对标, ocx 前端插件很慢, 没有cs产品线, bs 产品线插件被砍掉了 只用webgl 来做,不推荐用户cs ,深圳大数据量 70 完楼, lod控制逐渐输出,  导入服务器, webgl在性能展示,动态加载, 大面积跟友商测试根本不测, 因为不第三方,  上海外环 700平方公里, bs模式也是有演示效果, 视频展示.

上海建筑物密集的地方展示, 都可以的. 建筑物个数

上来就是问高端技术方面,没有

发布的速度很慢  slpk打包,server 端缓存,1比4 缓存, 后台集群, 700平方公里 精模, 并发多少数据需要测绘一下.并发10个用户, 上了多少台机器, 分布式的前端分布式, 数据是一份, 20多台分不同的应用场景,分分片, 不同场景,配不同的服务器,采购了4个p的存储, 数据无需多个存储.  三种数据库 可以有自己独有的集群效果.

测绘院,硬件,测绘保密网, 不做倾斜摄影,  如果挑剔的画,不会满意倾斜摄影, 因为会胡,  除非面积小,倾斜摄影效果就会比较完美, 跟南师大合作, 软件修磨,变成 slpk, 它在三维这块都是分包, 二维这块都是自己的, 高空飞, 申请飞行航道(大于120米),

以上是领导回答



cs用的不多, cs能够快速下载能够支持到我的, 但是在bs环境下可以.  如果只能用你的server 开发和上线不在一个环境,怎么解决, arcgic服务部署,到时候给你一份文档, 机器送修,没有文档.   地图服务发到服务端后,能不能持续改变. 数据源注册, 如果你有变化, 前端就会变化,放在, 有几类数据,比如三维场景数据不好做, 三维服务数据 

featureservice 可以, 动态业务, 二维切片.



dem是高层数据, 数字表面, 注册数据源.

layer, pro server 都可以, 推荐用哪个啊.

pg是一个黑匣子,server里面注册自己的数据. 集群模式等,分片.产品的角度自接给你.  开发版软件没有中文, 应用软件有中文.



arcgis pro专有名词.  js 4.9 默认用webgl 绘制, 比较快.  使用server去记算 . 

集群计算,多个任务同 , 同一个任务多个机器算 不可能.



明确一下前端代码写, 整个图形渲染的过程. 数据流程可以切成好几段.



图层layer  





后端批量插入比较好.  



不接受jl json 



后端可以有时代数据, 标准时间, 前端展示. 



前端bookmark  幻灯片场景转换点., (extend 比例尺 ok)  

写到底就是dc,都可以写

feature  data source  grafic layer



var alist = [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18];  function isIn(id) {                            for (var i = 0; i < 18 ; i++) {                                if (id == alist[i]) {                                    return true;                                }                            }                            return false;                        };                        var fid = $feature.id;                        if(isIn(fid)){                            var eq = $feature.education_quota;                            return 1;                        }                        return 1





"var fields = [
{ value: $feature.INDADMN_CY, alias: "Admin/Waste Mgmt" },
{ value: $feature.INDAGRI_CY, alias: "Agriculture" },
{ value: $feature.INDARTS_CY, alias: "Arts/Entertainment/Rec" },
{ value: $feature.INDCONS_CY, alias: "Construction" },
{ value: $feature.INDEDUC_CY, alias: "Educational Services" },
{ value: $feature.INDFIN_CY, alias: "Finance/Insurance" },
{ value: $feature.INDFOOD_CY, alias: "Accommodation/Food Svcs" },
{ value: $feature.INDHLTH_CY, alias: "Health Care" },
{ value: $feature.INDINFO_CY, alias: "Information" },
{ value: $feature.INDMANU_CY, alias: "Manufacturing" },
{ value: $feature.INDMGMT_CY, alias: "Management" },
{ value: $feature.INDMIN_CY, alias: "Mining" },
{ value: $feature.INDOTSV_CY, alias: "Other Services" },
{ value: $feature.INDPUBL_CY, alias: "Public Administration" },
{ value: $feature.INDRE_CY, alias: "Real Estate" },
{ value: $feature.INDRTTR_CY, alias: "Retail Trade" },
{ value: $feature.INDTECH_CY, alias: "Professional/Tech Svcs" },
{ value: $feature.INDTRAN_CY, alias: "Transportation" },
{ value: $feature.INDUTIL_CY, alias: "Utilities" },
{ value: $feature.INDWHTR_CY, alias: "Wholesale Trade" }
];
function map (array, callback){
var values = [];
for(var k in array){
values[k] = callback(array[k]);
}
return values;
}
function getFieldValueCallback (field){
return field.value;
}
function getFieldValues(fieldsArray){
return map(fieldsArray, getFieldValueCallback);
}
var fieldValues = getFieldValues(fields);
var winner = Max(fieldValues);
var total = Sum(fieldValues);
return (winner/total) * 100;"

貌似是支持vb语言库









​                                                var alist = [18,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17];                        function isIn(id) {                            for (var i = 0; i < 18 ; i++) {                                if (id == alist[i]) {                                    return true;                                }                            }                            return false;                        };                        var fid = $feature.id;                        if(isIn(fid)){                            var eq = $feature.education_quota;                            return eq;                        }                        return 1;                        























