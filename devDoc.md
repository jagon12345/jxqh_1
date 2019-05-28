#### 商家端权限：

—————
- 超级管理员（可添加、删除、查看管理员、操作员，拥有所有权限，即所有公司权限。可添加公司、门店，不把该权限交给公司管理员）
    - 管理员（权限列表—> 店长所有权限加 所有门店、公司的管理，可添加管理员）
        -  店长（权限列表—>员工所有权限加 查询门店业绩、财务报表、营业记录、会员删除、员工管理、日常开支、所有员工预约信息）
        -  员工（权限列表—>取单、挂单、查询员工业绩、会员卡充值、开卡、会员查询和增加、个人预约信息），默认具有员工权限。
        -  自定义（权限列表—>自定义）       注：管理员只能设置自己已有权限


    - 操作员（为管理员添加公司、门店以及其他操作）

根据权限列表返回菜单—> menu

—————
用户端完全独立，不设置权限。


#### 前端调用方式

我们做了一个JWT的认证模块:
(access token在以下代码中为'token'，refresh token在代码中为'rftoken')

首次认证
client -----用户名密码-----------> server

client <------token、rftoken----- server

access token存续期内的请求
client ------请求（携带token）----> server

client <-----结果----------------- server

access token超时
client ------请求（携带token）----> server

client <-----msg:token expired--- server

重新申请access token
client -请求新token（携带rftoken）-> server

client <-----新token-------------- server

rftoken token超时
client -请求新token（携带rftoken）-> server

client <----msg:rftoken expired--- server


大致是这样的，用的是jwt，登录后我会给个token,之后就用token请求

当token过期时，请求将返回401错误，并返回token expired信息，这时需要前端用rftoken请求新的token，服务端返回新的token之后，前端继续之前的用户请求

关于token的使用参考： https://ninghao.net/blog/5530

这里不同之处在于多加了一个rftoken，用于在token过期时请求新的token，不需要用户重新登录

如果用户登出，那就丢弃已存储的token和rftoken

用户长时间未操作，判断用户登出，参考：  https://blog.csdn.net/xiaoshihoukediaole/article/details/81327471
