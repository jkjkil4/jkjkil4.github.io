---
layout: post
title: LearnOpenGL 3 安装与创建窗口
date: 2024-07-22 13:11 +0800
categories: [教程, LearnOpenGL]
tags: [Python, OpenGL]
---

> LearnOpenGL CN 相关链接：<https://learnopengl-cn.github.io/01%20Getting%20started/03%20Hello%20Window/>
{: .prompt-info }

> 本节视频：<https://www.bilibili.com/video/BV1mVWDe6EM6?p=3>
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

# 渲染循环
while not glfw.window_should_close(window):
    glfw.poll_events()
    glfw.swap_buffers(window)

# 终止 GLFW
glfw.terminate()
```
