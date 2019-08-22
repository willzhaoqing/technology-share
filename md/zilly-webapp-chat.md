# webapp-chat问题：

1. API缺少文档，不清楚每个API的作用，需要一份完整的API文档
> 调用版本、参数
https://api.dev.tellusapp.com/api/v1/channels/`688759522320461046`
https://api.dev.tellusapp.com/api/v1/channels/preview?channel_ids[]=688759522320461046&channel_ids[]=688758682422619399&channel_ids[]=688759035206315268&channel_ids[]=687458175149944039&channel_ids[]=687458179974180070&channel_ids[]=687455410547076335
https://api.dev.tellusapp.com/api/v1/channels?exclude_from_timestamp=2019-08-07T12%3A11%3A40.769944Z&per_page=20

https://api.dev.tellusapp.com/api/v1/messages/media/?channel_id=688759522320461046

都是做什么的？跟IOS有什么区别

2. 流程说明
> 基本没有，后续和未来目标是什么。

3. 功能
+ 卡片
payload: {z_property_id: "438789808035727781", price: 575000, street_address: "711 W Wilson W",…}
bedrooms: 4
city: "Glendale"
is_bookmarked: false
mls_number: "12107361"
photo: "http://media.cdn-redfin.com/photo/58/bigphoto/792/316000792_1_2.jpg"
price: 575000
property_id: "438789808035727781"
role: null
role_status: null
state: "CA"
street_address: "711 W Wilson W"
z_property_id: "438789808035727781"
zip_code: "91203"

`新版的设计里，我找不到相对应的图片，介绍等字段`

## 解决方案
`onboarding`项目前端团队是通过QA了解流程，通过IOS的同事了解API作用和调用。非常花时间。
1. prd 文档    
2. API 文档，包括data-format说明  
3. 安全策略文档
4. 时间规划  
最好能有QA记录的buglist，更为全面。  
`需要PM翻译并整理的帮助`

## 聊天|IM 应有架构
+ 需要有ID|KEY，所有信息录入到某表或统一关联。
+ 信息需要保密，用户首先获取Token，再根据Token获取ACL(Access Control List)，信息可见与否需要判断；同理，信息类型（text,file:img,voice,vedio,card）
+ 文件下载地址的 过期，鉴权
+ file类型文件的缓冲、加载和CDN，存储在server还是client？


## 讨论和结果
> Paul整理，Pete跟进，整理文档

