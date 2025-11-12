# TexMetro Qt5 Rollback Compilation

I was able to compile the project on my macbook pro with an M2 Pro on macOs 15.6.1 using `qmake`.

### Modifying the build file
I settend the path for my glew installation and removed the AGL support, since it is now deprecated. 

To do so I modified:

```make
##### INCLUDE PATH #############################################################

INCLUDEPATH += $$VCGPATH $$VCGPATH/eigenlib



#### LIBS ######################################################################
...
```

to 

```make
##### INCLUDE PATH #############################################################
  INCLUDEPATH += $$VCGPATH $$VCGPATH/eigenlib
  macx {
      INCLUDEPATH += /opt/homebrew/Cellar/glew/2.2.0_1/include
  }
 
#### LIBS ######################################################################
  macx {
      LIBS += -L/opt/homebrew/Cellar/glew/2.2.0_1/lib
      LIBS += -lGLEW
 
      # Override Qt's OpenGL framework settings to exclude AGL
      QMAKE_LIBS_OPENGL = -framework OpenGL
      LIBS += $$QMAKE_LIBS_OPENGL
 }
```

