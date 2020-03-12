# 我的助手jarvis

> jarvis创建自开源项目[hubot](https://hubot.github.com/)，很多年前已经可以通过slack对话的形式帮助用户完成一些自动化任务，后期考虑召唤钢铁侠留给小蜘蛛的Edith.



## hubot语法模式

hubot能够做什么，取决于用户自定义的脚本赋予了它什么功能，了解下hubot的语法模式

```coffeescript
module.exports = (robot) ->
  # code here
```



### Hear & Respond

```coffeescript
module.exports = (robot) ->
  robot.hear /badger/i, (res) ->
    # your code here

  robot.respond /open the pod bay doors/i, (res) ->
    # your code here
```

两者功能一致，但是匹配方式不同，hear可以响应任何匹配文本的指令，而respond只响应【hubot昵称】+文本的模式，以jarvis为例，respond会响应" jarvis open the pod bay doors"

### Send & Reply

将结果写入res，返回给调用方:

```coffeescript
module.exports = (robot) ->
  robot.hear /badger/i, (res) ->
    res.send "Badgers? BADGERS? WE DON'T NEED NO STINKIN BADGERS"

  robot.respond /open the pod bay doors/i, (res) ->
    res.reply "I'm afraid I can't let you do that."

  robot.hear /I like pie/i, (res) ->
    res.emote "makes a freshly baked pie"
```

send（emote）和reply功能基本一致，不同的是reply会带上调用方名称



## Messages to a room or user

```coffeescript

```



## Capturing data

如果输入文本 “HAL: open the pod bay doors”, 那么 `res.match[0]` 的结果是 “open the pod bay doors”,  `res.match[1]`的结果是“pod bay”：

```coffeescript
robot.respond /open the (.*) doors/i, (res) ->
    doorType = res.match[1]
    if doorType is "pod bay"
      res.reply "I'm afraid I can't let you do that."
    else
      res.reply "Opening #{doorType} doors"
```



## Making HTTP calls

使用 `robot.http`模块进行http调用



### 异常处理模块



















































1. 启动redis docker

   docker run --name jarvis-brain  -d redis

84 2 

70 1



84 

70