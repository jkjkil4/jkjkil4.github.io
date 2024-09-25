---
layout: post
title: LearnOpenGL 8 更多属性（彩色三角形）
date: 2024-09-09 09:15 +0800
categories: [教程, LearnOpenGL]
tags: [python, opengl]
---

> LearnOpenGL CN 相关链接：<https://learnopengl-cn.github.io/01%20Getting%20started/05%20Shaders/>
{: .prompt-info }

> 本节视频：<https://www.bilibili.com/video/BV1CkxuexEeQ?p=3>
{: .prompt-info }

## 本节代码

```py
# 导入需要的库
import math

import glfw
import moderngl as mgl
import numpy as np

# 初始化 GLFW
if not glfw.init():
    raise Exception('GLFW出错')

# 创建窗口
window = glfw.create_window(800, 600, 'LearnOpenGL', None, None)
if not window:
    glfw.terminate()
    raise Exception('window出错')

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

# 着色器程序
vertex_shader = '''
#version 330 core

in vec3 in_vert;
in vec3 in_color;

out vec3 v_color;

void main()
{
    gl_Position = vec4(in_vert, 1.0);
    v_color = in_color;
}
'''

fragment_shader = '''
#version 330 core

in vec3 v_color;

out vec4 FragColor;

void main()
{
    FragColor = vec4(v_color, 1.0);
}
'''

prog = ctx.program(vertex_shader, fragment_shader)

# 顶点数据
vertices = np.array([
     # 位置            # 颜色
     0.5, -0.5, 0.0,  1.0, 0.0, 0.0,    # 右下
    -0.5, -0.5, 0.0,  0.0, 1.0, 0.0,    # 左下
     0.0,  0.5, 0.0,  0.0, 0.0, 1.0     # 顶部
], dtype='f4')

vbo = ctx.buffer(vertices.tobytes())

# 顶点数组对象
vao = ctx.vertex_array(prog, vbo, 'in_vert', 'in_color')

# 渲染循环
while not glfw.window_should_close(window):
    # 输入
    process_input(window)

    # 渲染指令
    ctx.clear(0.2, 0.3, 0.3)
    vao.render(mgl.TRIANGLES)

    # 处理事件、交换缓冲
    glfw.poll_events()
    glfw.swap_buffers(window)

# 终止 GLFW
glfw.terminate()
```
