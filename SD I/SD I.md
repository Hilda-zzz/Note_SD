## 8.26
### Grade
	8 assignments at 8% each
	9% quizzes
	17% final exam
	10% participation
## 8.28
### Perforce


### Construction
1. starship.exe
2. game.vcxproj
	1. (code)
	2. asteroid
	3. bullet
	4. beetled
3. Engine.lib
	1. math
	2. graphics
	3. audio
	4. input
	5. network
4. Engine.vcxproj
	1. code
starship.sln merge them together to form a game
5. MathUnitTests.exe
	1. MathUnitTests.sln
6. Protogame2D.exe
7. Protogame3D.exe
### Engine features
1. no specific
2. high quality
### Why C++?
1. cross platform
2. control over memory management
3. compile to native exe
4. control over to compile speed
5. widely adopted
6. control over performance
7. flexible
## 8.30
### Game Frame
FPS= Frames per second
	Swap Buffers(OpenGL)
	Present(DirectX)
	Double Buffered screen
	60FPS=60Hz
	1 Frame=1/60 sec=16.67ms
1 Frame is:
	Engine Begin Frame
	App -> Update (a.k.a Tick) // Change things, Do not Draw
	App -> Render const  // Draw things, Do not Change
	Engine -> End Frame
	Present/ Swap Buffers
### OpenGL
TheApp_Render() bkg color
CLIENT_ASPECT window aspect



compile optimization
(in release but not debug)
release version will be faster

## 9.4 The C++ Build Process
1. Preprocessor - "word processing"
		#include #define
		a copy paste process
2. compilation
		each cpp file is compiled separately into a .obj
3. linking 
		combine together all obj and lib files into exe

### Declaration---"Advertisement for sth else"
"there will be somewhere else, it's a promise that you will find it future"
1. 常见链接错误，找不到声明过的东西
2. 重复找到多次同名函数
（可以声明很多次同一个函数，但是只能定义同一个）
### Definition---"the things"
definition never used in hpp, cause it may be included more than once



``` cpp
extern int g_someGloabel;
int g_someGLoabel=42;
constexpr int g_someGlobal=42;
#define g_someGlobal  42;
enum
{
	//same to constexpr
};
```