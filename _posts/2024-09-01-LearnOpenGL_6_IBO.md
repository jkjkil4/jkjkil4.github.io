---
layout: post
title: LearnOpenGL 6 索引缓冲对象
date: 2024-09-01 19:23 +0800
categories: [教程, LearnOpenGL]
tags: [python, opengl]
---

> LearnOpenGL CN 相关链接：<https://learnopengl-cn.github.io/01%20Getting%20started/04%20Hello%20Triangle/>
{: .prompt-info }

> 本节视频：<https://www.bilibili.com/video/BV1CkxuexEeQ?p=1>
{: .prompt-info }

## 本节代码

```py
# 导入需要的库
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

void main()
{
    gl_Position = vec4(in_vert.x, in_vert.y, in_vert.z, 1.0);
}
'''

fragment_shader = '''
#version 330 core

out vec4 FragColor;

void main()
{
    FragColor = vec4(1.0, 0.5, 0.2, 1.0);
}
'''

prog = ctx.program(vertex_shader, fragment_shader)

# 顶点数据
vertices = np.array([
    0.5, 0.5, 0.0,      # 右上角
    0.5, -0.5, 0.0,     # 右下角
    -0.5, -0.5, 0.0,    # 左下角
    -0.5, 0.5, 0.0      # 左上角
], dtype='f4')

vbo = ctx.buffer(vertices.tobytes())

indices = np.array([
    0, 1, 3,    # 第一个三角形
    1, 2, 3     # 第二个三角形
], dtype='i4')

ibo = ctx.buffer(indices.tobytes())

# 顶点数组对象
vao = ctx.vertex_array(prog, vbo, 'in_vert', index_buffer=ibo)

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
