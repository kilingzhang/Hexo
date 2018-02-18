---
title: Web QQ协议分析(三)接收消息
tags:
  - WebQQ协议
  - QQ机器人
  - PHP
id: 142
categories:
  - PHP
  - QQRobot
  - SmartQQ
  - 后端
date: 2017-02-04 18:12:31
---

# 轮训消息

请求方式

 `Post`

url

 `http://d1.web2.qq.com/channel/poll2` 
<!--more-->

referer 
`http://d1.web2.qq.com/proxy.html?v=20151105001&amp;callback=1&amp;id=2`

参数

    r={
        "ptwebqq": "{$ptwebqq}",
        "clientid": 53999199,
        "psessionid": "{$psessionid}",
        "key": ""
    }

    `</pre>

    response

    ### 好友消息

    <pre>`{
        "result": [
            {
                "poll_type": "message",
                "value": {
                    "content": [
                        [
                            "font",
                            {
                                "color": "000000",
                                "name": "微软雅黑",
                                "size": 10,
                                "style": [
                                    0,
                                    0,
                                    0
                                ]
                            }
                        ],
                        "好啊"
                    ],
                    "from_uin": 3785096088,
                    "msg_id": 25477,
                    "msg_type": 0,
                    "time": 1450686775,
                    "to_uin": 931996776
                }
            }
        ],
        "retcode": 0
    }

    `</pre>

    `poll_type`为`message`表示这是个好友消息。`from_uin`是用户的编号，可以用于发消息，但不是`qq`号。`to_uin`是接受者的编号，同时也是`QQ`号。`time`为消息的发送时间，`content[0]`为字体 其余字面意思
    很值得注意的是，webQQ中所有的通用号码不是QQ号而是各个`uin` 而且uin的号码不是固定的 所以不能保存uin来确定某个人 必须每次都要请求下获取个人信息接口查询QQ确认身份

    ### 讨论组消息

    <pre>`{
        "result": [
            {
                "poll_type": "discu_message",
                "value": {
                    "content": [
                        [
                            "font",
                            {
                                "color": "000000",
                                "name": "微软雅黑",
                                "size": 10,
                                "style": [
                                    0,
                                    0,
                                    0
                                ]
                            }
                        ],
                        "好啊",
                    ],
                    "from_uin": 2322423201,
                    "did": 2322423201,
                    "msg_id": 50873,
                    "msg_type": 0,
                    "send_uin": 3680220215,
                    "time": 1450687625,
                    "to_uin": 931996776
                }
            }
        ],
        "retcode": 0
    }

    `</pre>

    ### 群消息

    <pre>`{
        "result": [
            {
                "poll_type": "group_message",
                "value": {
                    "content": [
                        [
                            "font",
                            {
                                "color": "000000",
                                "name": "微软雅黑",
                                "size": 10,
                                "style": [
                                    0,
                                    0,
                                    0
                                ]
                            }
                        ],
                        "好啊",
                    ],
                    "from_uin": 2323421101,
                    "group_code": 2323421101,
                    "msg_id": 50873,
                    "msg_type": 0,
                    "send_uin": 3680220215,
                    "time": 1450687625,
                    "to_uin": 931996776
                }
            }
        ],
        "retcode": 0
    }

    `</pre>

    同理 
    `group_code`和`from_uin`都为群的编号，可以用于发群消息，但不是群号。`send_uin`为发信息的用户的编号。 `did`为讨论组的编号 ,其他的字段和上面的相同。

    ### 所有可能出现的消息类型

    <pre>`            if ($news['poll_type'] == 'message') {
                    //好友消息
                    $this-&gt;personalMsg($news, $rows);
                } elseif ($news['poll_type'] == 'group_message') {
                    //群消息
                    $this-&gt;groupMsg($news, $rows);
                } elseif ($news['poll_type'] == 'sys_g_msg') {
                    //加群验证消息
                    $this-&gt;sysGroupMsg($news, $rows);
                } elseif ($news['poll_type'] == 'sess_message') {
                    //临时会话消息
                } elseif ($news['poll_type'] == 'discu_message') {
                    //讨论组消息
                    $this-&gt;didMsg($news, $rows);
                } elseif ($news['poll_type'] == 'buddies_status_change') {
                    //好友状态改变提醒
                } 

详情可以看我写的php版本的[SmartQQ](https://github.com/slight-sky/SmartQQ)类
因为群消息处理不是webQQ的协议 所以不详解，研究的可以看源码

## 注意

*   以为是才用poll轮询的方式获取消息，所以服务端收到这个请求后，如果没有新消息，会一直保持住链接，所以遇到ReadTimeout异常是正常的*   如果消息内容为表情，content[1]的内容就不是String类型了，而是一个JSONArray类型，里面有表情的编号*   这个请求有时候会返回retcode的值为103，此时需要登录[Smart QQ](http://w.qq.com "SmartQQ")，确认能收到消息后点击设置-退出登录，就会恢复正常了