# Notes on OpenGL rendering
These notes are compatible with 3.3 and upwards.

Most of the time OpenGL works in a way that first you tell it what object you want to command and after that you give commands to that object. It is different from OOP where you have you object which you want to command. Following example is fabricated
 
```c
/*
 * Commanding OOP style
 * We use constructor or method to return us new
 * instance of object
 * */
 BufferObject bo = glGenBuffer(1);

/*
 * Commading OpenGL style
 * First we declare something to hold our object
 * then we pass that as and argument to function
 * */
 unsigned int bo;
 glGenBuffers(1, bo);
```

## Buffer objects


When rendering anything following things are needed:
- Vertex Buffer Object, aka VBO
- Vertex Array Object, aka VBA
- Frame Buffer Object, aka FBO

### Creating buffer object
First we need to create our buffer. This is done using following command
```c
GLuint bo;
glGenBuffers(1, &bo);
```
Command ```glGenBuffers``` can be used to initialize multiple buffers at once by specifying number of buffers as first parameter and pointer to start of interger array where you want to store you buffers.

All OpenGL objects are pretty much just integers, that you use throughout OpenGL program to interact with them. So store that ```GLuint``` some place you can find it later when you need it.

It might make it easier if you think that ```bo``` name of you *buffer object*, and you just call her by that name when you need her.

### Binding buffer object
As now we have our buffer initialized,  we need to *bind* buffer object so OpenGL know that all next buffer related command are addressed to that precise VBO.
```c
glBindBuffer(GL_ARRAY_BUFFER, bo);
``` 
After that OpenGL knows that when ever we call any buffer related function with ```GL_ARRAY_BUFFER``` parameter that will target ```bo```.

To tell OpenGL to forget about ```bo``` command:
```c
glBindBuffer(GL_ARRAY_BUFFER, 0);
```
Also commanding ```glBindBuffer``` with same first parameter but different second parameter also tells OpenGL to forget about firs buffer object.

```glGenBuffers``` and ```glbindBuffer``` are common for subsequent buffer commands.  

### Vertex buffer object
As it is known that 3D stuff consists of vertices. Vertices can be easily defined for example

```c
float vertices[] = {
    -1.0f, -1.0f, 0.0f,
     1.0f, -1.0f, 0.0f,
     0.0f,  1.0f, 0.0f    
};
```
But we need somekind of medium to transfer vertices to GPU. **Vertex buffer object** (from now on VBO) is medium to transfer vertices to GPU. 

Let's consider following:
```c
GLuint vbo;
glGenBuffers(1, &vbo);

glBindBuffer(GL_ARRAY_BUFFER, vbo);

float vertices[] = {
    -1.0f, -1.0f, 0.0f,
     1.0f, -1.0f, 0.0f,
     0.0f,  1.0f, 0.0f 

glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);
```
What we have done here is that we commanded OpenGL to generate one **vertex buffer object** (```vbo```), which we then bind, so OpenGL knows this buffer object is active and all subsequential commands targetting buffers are targeted to  ```vbo```.

After binding we declare three points in space, which can be interpret as single a triangle. We then tell OpenGL to populate ```vbo``` with ```vertices```. We have successfully created first vertex buffer object with information about simple polygon.

## Array objects
Array objects are containers for buffer objects. One array object contains one or more buffer objects. Array object are generated and bound in same manner as buffer objects, except that we have different functions to generate and bind different kinds of array objects. 

**Vertex array object** is used to contain vertex buffer objects. To generate vertex array object, aka VAO, we use following command:
```c
GLuint vao;
glGenVertexArrays(1, &vao);
```
Just like with buffer objects, you have to keep track of ```vao``` and use that to reference you vertex array object.

Just like with buffer objects, we have to bind array object to use it. To bind vertex array object we use following command:
```c
glBindVertexArray(vao);
```
This tells OpenGL that subsequential commands considering vertex arrays are targetted to ```vao```
