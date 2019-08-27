---
layout: post
title: Hello Window in OpenGL
categories:
  - GPU and GPU Programming
---

This is a full code of Hello Window from the tutorial "Learn OpenGL" [^LOGL].

```c++
#include <iostream>

#include <glad/glad.h>
#include <GLFW/glfw3.h>

void framebuffer_size_callback(GLFWwindow* window, int width, int height);
void processInput(GLFWwindow *window);

int main() {
	// Intialize GLFW
	glfwInit();

	// Configure GLFW (v3.3)
	glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
	glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);
	glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);

	// Create a window object
	GLFWwindow* window = glfwCreateWindow(800, 600, "LearnOpenGL", NULL, NULL);
	if (window == NULL) {
		std::cout << "Failed to create GLFW window" << std::endl;
		glfwTerminate();
		return -1;
	}

	// Make our window the main context on the current thread
	glfwMakeContextCurrent(window);

	// Initialize GLAD
	if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress)) {
		std::cout << "Failed to initialize GLAD" << std::endl;
		return -1;
	}

	// Set size of the rendering window (dimensions)
	glViewport(0, 0, 800, 600);

	// Register a callback function on the window that gets called
	// each time the window is resized
	glfwSetFramebufferSizeCallback(window, framebuffer_size_callback);

	// Render loop that keeps on running until we tell GLFW to stop
	while(!glfwWindowShouldClose(window)) {
		// Check input close window if it is the escape key
		processInput(window);

		// Rendering commands here
		// Set a color to clear the screen with (state-setting function)
		glClearColor(0.2f, 0.3f, 0.3f, 1.0f);
		// Clear the screen with the color (state-using function)
		glClear(GL_COLOR_BUFFER_BIT);

		// Swap the color buffer and show output to the screen
		glfwSwapBuffers(window);

		// Check if any events are triggered
		glfwPollEvents();
	}

	// Clean/delete all GLFW's resources
	glfwTerminate();

	return 0;
}

void framebuffer_size_callback(GLFWwindow* window, int width, int height) {
	// Update width and height
	glViewport(0, 0, width, height);
}

void processInput(GLFWwindow *window) {
	// Check whether the user has pressed the escape key and close GLFW
	if (glfwGetKey(window, GLFW_KEY_ESCAPE) == GLFW_PRESS)
		glfwSetWindowShouldClose(window, true);
}
```

The code is compiled using the following command.
```shell
$ g++ hello_window.cpp glad.c -o hello_window -lglfw -lGL -lm -lXrandr -lXi -lX11 -lXxf86vm -lpthread -ldl -lXinerama -lXcursor
$ ./hello_window
```

[^LOGL]: <https://learnopengl.com/Getting-started/Hello-Window>
