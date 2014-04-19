title: 2D/3D Graphics with Python on Mobile Platforms

Why?

* Fun
* Education

Kivy

* Ports python onto ARM Android/iOS
* Cython wrappers for OpenGL|ES

Example:

    #!python
    from kivy.core.window import Window

    Window.canvas

    from kivy import graphics

Vertex Instructions

* Shapes

Context Instructions

* Color
* Rotation
* Scale
* Translate
* Bind Textures

Example:
    
    #!python
    with widget.canvas:
         Color(1, 0, 0)
         Rectangle(size=(100, 100), pos=(20, 20))
         Color(0, 1, 0)

###Textures

    #!python
    from kivy.core.image import Image

    texture = Image("").texture
    size = texture.size

Ignifuga - based on SDL

github.com/nskrypnik