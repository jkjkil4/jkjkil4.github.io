---
layout: post
title: LearnOpenGL 7 着色器
date: 2024-09-06 12:08 +0800
categories: [教程, LearnOpenGL]
tags: [python, opengl]
---

> LearnOpenGL CN 相关链接：<https://learnopengl-cn.github.io/01%20Getting%20started/05%20Shaders/>
{: .prompt-info }

> 本节视频：暂未发布
{: .prompt-info }

### 顶点着色器向片段着色器传递数据（渲染暗红色三角形）

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

out vec4 v_color; // 指定一个传递给片段着色器的输出

void main()
{
    gl_Position = vec4(in_vert, 1.0);
    v_color = vec4(0.5, 0.0, 0.0, 1.0); // 把输出变量设置为暗红色
}
'''

fragment_shader = '''
#version 330 core

in vec4 v_color;  // 从顶点着色器传来的输入变量（名称相同，类型相同）

out vec4 FragColor;

void main()
{
    FragColor = v_color;
}
'''

prog = ctx.program(vertex_shader, fragment_shader)

# 顶点数据
vertices = np.array([
    -0.5, -0.5, 0.0,
     0.5, -0.5, 0.0,
     0.0,  0.5, 0.0
], dtype='f4')

vbo = ctx.buffer(vertices.tobytes())

# 顶点数组对象
vao = ctx.vertex_array(prog, vbo, 'in_vert')

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

### 使用 uniform 传递数据（渲染亮度变化的绿色三角形）

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

void main()
{
    gl_Position = vec4(in_vert, 1.0);
}
'''

fragment_shader = '''
#version 330 core

uniform vec4 ourColor;   // 在OpenGL程序代码中设定这个变量

out vec4 FragColor;

void main()
{
    FragColor = ourColor;
}
'''

prog = ctx.program(vertex_shader, fragment_shader)

# 顶点数据
vertices = np.array([
    -0.5, -0.5, 0.0,
     0.5, -0.5, 0.0,
     0.0,  0.5, 0.0
], dtype='f4')

vbo = ctx.buffer(vertices.tobytes())

# 顶点数组对象
vao = ctx.vertex_array(prog, vbo, 'in_vert')

# 渲染循环
while not glfw.window_should_close(window):
    # 输入
    process_input(window)

    # 渲染指令
    ctx.clear(0.2, 0.3, 0.3)

    time_value = glfw.get_time()
    green_value = (math.sin(time_value) / 2.0) + 0.5
    prog['ourColor'] = (0.0, green_value, 0.0, 1.0)
    vao.render(mgl.TRIANGLES)

    # 处理事件、交换缓冲
    glfw.poll_events()
    glfw.swap_buffers(window)

# 终止 GLFW
glfw.terminate()
```
