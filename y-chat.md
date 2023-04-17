# y-chat

**暂时测试过微信小程序和h5,安卓app**

**后面工作不忙有空的时候会继续更新**

**作者qq: 1763637512  不懂的可以私聊我 备注uniapp**

## y-chat --- Attributes

| userId          | 当前用户id                                                   |
| --------------- | ------------------------------------------------------------ |
| messageList     | 消息列表(必传)                                               |
| updateList      | 更新数据列表(推荐更新数据时使用这个属性更新, 但是也兼容了自己更改messageList) |
| defaultOptions  | 可自定义messageList的数据名,messageList配置项                |
| tagOptions      | 标签配置项(消息列表中传了tagLabel时必传)                     |
| useRefresh      | 是否启用下拉加载(默认为true)                                 |
| iconSize        | 图标大小                                                     |
| sheetList       | 自定义功能列表,支持自定义图标                                |
| retainSheet     | 是否保留默认功能(默认为true)                                 |
| useScrollBottom | 是否启用点击+号时聊天列表移动到最下方(默认为true)            |
| intervalTime    | 显示时间的间隔 单位/毫秒                                     |
| defaultAvator   | 默认头像(当前没有头像时默认显示的头像)                       |

## messageList

| userId               | 发消息用户的Id                                               |
| -------------------- | ------------------------------------------------------------ |
| msgId                | 消息Id,主要用来滑动列表                                      |
| name                 | 用户名称                                                     |
| message              | 当前消息内容                                                 |
| img                  | 发送的图片内容                                               |
| time(时间戳 => 毫秒) | 当前消息发送时间(默认两次消息时间差大于5分钟显示一次时间) <间隔时间可以通过intervalTime自定义> |
| avator               | 头像                                                         |
| tagLabel             | 当前聊天的标签属性,使用tagOptions自定义                      |

## updateList

如果是对象的形式,那就是发送消息, 如果是数组那就是历史消息

发送消息之后如果有返回当前发送消息详细信息可以以对象的形式赋值给updateList, 组件会自动更新出发送的数据, 可不不用自己更新messageList

在下拉加载历史消息时,将每次获取到的历史消息数组放到updateList中会自动更新出历史消息数组

当然这些也都可以自己去更改messageList解决, 但是我更推荐这种方法, 我都写好了,为什么你还要自己写呢

## defaultOptions 

| userId   | 配置messagfeList中的userId的对应key   |
| -------- | ------------------------------------- |
| msgId    | 配置messagfeList中的msgId的对应key    |
| name     | 配置messagfeList中的msgId的对应key    |
| message  | 配置messagfeList中的message的对应key  |
| img      | 配置messagfeList中的img的对应key      |
| time     | 配置messagfeList中的time的对应key     |
| avator   | 配置messagfeList中的avator的对应key   |
| tagLabel | 配置messagfeList中的tagLabel的对应key |

## tagOptions

**对象包对象的形式, key代表权限名, value是一个配置对象就比如**

```javascript
{
    'xx':{
        text: '管理员', //所代表的文字
        bgColor: '#ff4100',	//背景和边框颜色
        color: '#fff'	//字体颜色
    },
    ...
}
```

**消息列表里的tagLabel就对应该对象里的key**

## sheetList

**自定义功能统一放到加号下,类似微信**

| name     | 自定义功能名称                            |
| -------- | ----------------------------------------- |
| icon     | 自定义功能图标(只支持uview官方图标库)     |
| img      | 自己上传图片(优先展示)                    |
| funLabel | 自定义传出去的方法名称(传sheetList时必传) |

## 方法

| send       | 发送消息,需要自己更新messageList中的数据 | 返回当前输入框数据     |
| ---------- | ---------------------------------------- | ---------------------- |
| onRefresh  | 下拉加载,                                | 返回停止加载动画的方法 |
| playPhoto  | 相册回调                                 | 返回选择的图片等信息   |
| playCemera | 拍摄回调                                 | 返回拍摄的照片等信息   |
| tapAvator  | 点击头像事件                             | 反向当前消息的详细信息 |



**onRefresh方法可以将请求获取的数据给到updateList, 每次赋值新的就好, 不用保留上一次的updateList, 会自动刷新出历史记录, 具体可以看示例,或者加我,备注uniapp**

**组价内部有一个scrollBottom方法, 如果组件没有自动滑到最后一条数据, 可以获取使用一次这个方法**



