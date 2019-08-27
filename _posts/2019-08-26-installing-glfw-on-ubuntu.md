---
layout: post
title: Installing GLFW on Ubuntu (+ GLAD)
categories:
  - GPU and GPU Programming
---

```shell
sudo apt-get install libglfw3
sudo apt-get install libglfw3-dev
```

Then we can run the example program in GLFW homepage!

```c
// test.c
#include <GLFW/glfw3.h>

int main(void)
{
    GLFWwindow* window;

    /* Initialize the library */
    if (!glfwInit())
        return -1;

    /* Create a windowed mode window and its OpenGL context */
    window = glfwCreateWindow(640, 480, "Hello World", NULL, NULL);
    if (!window)
    {
        glfwTerminate();
        return -1;
    }

    /* Make the window's context current */
    glfwMakeContextCurrent(window);

    /* Loop until the user closes the window */
    while (!glfwWindowShouldClose(window))
    {
        /* Render here */
        glClear(GL_COLOR_BUFFER_BIT);

        /* Swap front and back buffers */
        glfwSwapBuffers(window);

        /* Poll for and process events */
        glfwPollEvents();
    }

    glfwTerminate();
    return 0;
}
```

```shell
g++ -pthread -o test test.c -lglfw -lGLU -lGL -lXrandr -lXxf86vm -lXi -lXinerama -lX11 -lrt -ldl
./test
```

Addition: GLAD can be installed with the following commands.
```shell
git clone https://github.com/Dav1dde/glad.git
cd glad
cmake ./
make
sudo cp -a include /usr/local/
```
