---
layout: post
title: LearnOpenGL 4 处理输入与背景颜色
date: 2024-07-27 10:04 +0800
categories: [教程, LearnOpenGL]
tags: [Python, OpenGL]
---

> LearnOpenGL CN 相关链接：<https://learnopengl-cn.github.io/01%20Getting%20started/03%20Hello%20Window/>
{: .prompt-info }

> 本节视频：<https://www.bilibili.com/video/BV1mVWDe6EM6?p=4>
{: .prompt-info }

## 本节代码

```py
# 导入需要的库
import glfw
import moderngl as mgl

# 初始化 GLFW
if not glfw.init():
    raise Exception('GLFW出错')

# 创建窗口
window = glfw.create_window(800, 600, 'LearnOpenGL', None, None)
if not window:
    glfw.terminate()
    raise Exception('窗口出错')

# 获得上下文
glfw.make_context_current(window)
ctx = mgl.create_context()

# 视口
def framebuffer_size_callback(window, width, height):
    ctx.viewport = (0, 0, width, height)

glfw.set_framebuffer_size_callback(window, framebuffer_size_callback)

# 处理输入
def process_input(window):
    if glfw.get_key(window, glfw.KEY_ESCAPE) == glfw.PRESS:
        glfw.set_window_should_close(window, True)

# 渲染循环
while not glfw.window_should_close(window):
    # 输入
    process_input(window)

    # 渲染指令
    ctx.clear(0.2, 0.3, 0.3)

    # 处理事件、交换缓冲
    glfw.poll_events()
    glfw.swap_buffers(window)

# 终止 GLFW
glfw.terminate()
```
