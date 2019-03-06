# SDK 集成问题

本部分主要说明 SDK 集成前、后常见的一些问题。若不包含您遇到的情况，可以申请加入易观开发者 QQ 交流群：**474086078**。描述清楚现象，会有技术同学协助定位问题并帮助解决。

![QQ群](https://imguserradar.analysys.cn/fangzhou/img/2018/10/201810231522207133.png)

### Q1. 易观方舟当前支持哪种埋点方式？代码埋点和无埋点有什么区别? 

易观方舟定位是帮助企业开展用户精细化运营，目前只开放代码埋点。

**代码埋点和无埋点的区别：**

区别 |无埋点 |代码埋点
---|---|---
是否需要添加基础代码 |必不可少 |必不可少 
部署方式 |业务人员在页面点选| 需要前端工程师协助埋点
历史数据回溯 |可以| 埋点前数据不可回溯
事件属性数据采集|只能采集到用户交互数据，不支持采集属性数据，如果需要仍然需要埋点辅助| 可以添加多个事件属性，比如点击支付事件，可以添加支付订单、订单金额、支付方式等等
适用企业 |基础的指标监测|追求精细化运营，有多维度深入分析的需求

**总结：**
1. 埋点和无埋点没有绝对的优劣，取决于业务分析需求；
2. 无埋点并非完全不加代码，仍然需要埋基础代码；
3. 无埋点更准确的叫法应该是可视化事件部署；
4. 无埋点适用于只是查看基础指标的分析需求，对于追求精细化运营的企业来说，数据采集不全面的无埋点并不能解决问题。

### Q2. 从集成到看到回数通常需要多长时间？

调试阶段，开启 Debug 模式可以实时查看回传数据。

应用正式发布后，关闭 Debug 模式，数据会在应用打开关闭时上传，数据结果会实时展现，在事件分析中可以按分钟、按小时查看，最多延迟 10分钟；按日汇总当天数据，最多会延迟 2个小时。

### Q3. Debug 模式有什么作用？

Debug 模式下，SDK 会记录很多调试信息，日志会实时上传，更方便验证集成的正确性。

### Q4. 集成后 SDK 后会造成应用缓慢，影响用户体验吗？

不会。

### Q5. SDK 数据上传机制是什么样的？

无论Debug和非Debug模式，都是实时上传。

### Q6. 测试数据是否可以清除呢？

目前不支持单独的清除测试数据，但是可以通过新建测试应用，来验证集成过程，在正式发布之前，新建的正式应用，替换相应的 AppKey即可。

### Q7. 页面上看不到日志，是什么情况？

请先自行通过以下问题排查：

1. 是否按照集成文档正确集成，开启了 Debug 模式？只有 Debug 下，上传的日志才会显示在页面上，用于校验日志的完整性和正确性；

2. 是否在真机上运行了集成 SDK 的应用？点击了相应埋点的位置？

3. 当前是否有网络？无网络状态下的日志会缓存在本地，在有网络的时候，再发送;

    注：WiFi 或 蜂窝网皆可，部分 WiFi 代理的情况，可能会导致日志不能实时上传，请切换到蜂窝网尝试。
  
4. 是否打开了页面上的自动刷新按钮？或者手动刷新查看是否有日志；
  
5. 当前设备的时间是否与网络同步？如果设备时间滞后的话，可能会导致日志显示在最下方，误以为未上传。
  
6. 如果还是看不到日志，请加入易观开发者 QQ 交流群：474086078，会有技术同学协助定位问题。

### Q8. 回传日志列表中出现红色的日志标识什么？

事件/属性 ID 不能用字母、数字和下划线之外的字符，当 SDK 埋点的时候包含了其他字符，就会标红提示，开发人员需要根据提示修改埋点。

![日志报错](http://imguserradar.analysys.cn/fangzhou/img/2017/12/201712011406164974.png)

### Q9. 如何验证埋点是不是完整、准确？

可以元事件管理／用户属性管理中，查看每个事件和属性的回传状态，有三种情况：

![回传状态](http://imguserradar.analysys.cn/fangzhou/img/2017/12/201712011406168655.png)

**√** 表示当前事件/属性的数据已回传，集成正确；

**×** 表示上传的属性值数据类型错误，例如自定义的 商品金额 属性，其属性值应该是100、200这样的数值型，实际上传的数据如果毛衣、羽毛球等这样的字符串，就会识别为错误；需检查埋点代码；

**？** 表示未收到相应数据，需确定是否进行了相应的操作进行数据上传，或者检查代码是否集成

### Q10.  如果没有事先在方舟页面上自定义埋点事件和属性，如何验证实际埋点的情况？

系统会根据回传日志，自动匹配出实际埋点的事件和属性，确认显示名称和属性值类型后，即可加入到计算当中。