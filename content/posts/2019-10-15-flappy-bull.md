---
title: Flappy Bull!!
author: ''
date: '2019-10-15'
slug: flappy-bull
categories: []
tags:
  - Study
description: ''
externalLink: ''
series: []
---
emsp;网上冲浪的时候，偶尔发现Pyhton的一个开源包‘pyganme’，使用方法也比较简单，参考一下[介绍文档](https://github.com/pygame/pygame)后遂自己写了个‘flappy bull’的代码，哈哈，没错，就是‘flappy bull’！！

emsp;win下通过命令行`pip install pygame`安装源包，之后导言区就导入`import play`即可应用。

参考了一些帖子，源代码如下（可[下载](https://raw.githubusercontent.com/HankPPeng/HankPeng.com/master/images/flappy_bull.7z)）：

```
# flappy bull
import play

# using the bull image to create the "bull"
bull = play.new_image(
    image='bull.png',
    y=0,
    x=0,
    angle=0,
    size=100,
    transparency=100
    )

# adding gravity and lots of other physics to our bull
bull.start_physics(bounciness=0.4)

boxes = []

@play.repeat_forever
def do():
    if play.key_is_pressed('up', 'w'):
        # if the up key is pressed, make the bird jump
        bull.y += 6.5

    for box in boxes:

        # if the box is out of the screen, lets delete it
        if (box.x < (play.screen.left - 50)):
            boxes.remove(box)
            box.remove()

        # detect if the bird is touching any box
        if bull.is_touching(box):
            box.color = "red"

        # make the box move to the left
        box.x -= 1

@play.repeat_forever
async def block():
    # height of the top block
    top = play.random_number(lowest=300, highest=500)
    # height of the bottom block
    bottom = play.random_number(lowest=300, highest=500)

    # creating the top box of width 100, emerging from behind the current screen
    boxes.append(
        play.new_box(
            color="green",
            y=play.screen.top,
            x=play.screen.right + 50,
            width=50,
            height=top))
    # creating the bottom box of width 100, emerging from behind the current screen
    boxes.append(
        play.new_box(
            color="blue",
            y=play.screen.bottom,
            x=play.screen.right + 50,
            width=50,
            height=bottom))

    # creating the next box after a random duration between 2 and 5 seconds
    await play.timer(seconds=play.random_number(1.0, 4.0))


play.start_program()
```
emsp;运行之后键盘的上方向键即可让‘bull’动起来，碰到柱子就会变成红色。当然，参考以往火热的‘flappy bird’的话就需要添加更多的功能，如分数统计和死亡重新开始等，这些有时间再实现。

![flappy bull](https://raw.githubusercontent.com/HankPPeng/HankPeng.com/master/images/flappy_bull.png)
