
<a href="{{{rel_path}}}index.html" id="logo"><img src="{{{rel_path}}}images/logo.png" /></a>

<div class="topmenu">
[about](#about) . [libraries](#libs) ![github]({{{rel_path}}}/images/github.png)  [home](https://snowkit.github.io/linc) . [discuss](https://github.com/snowkit/linc/issues/)
</div>

---

<br/>
<a name="about"></a> 
##What is linc?

**linc** is **collection of libraries** for the **Haxe c++** target.

> “Low-level Interfaces Native Collection”


&nbsp;

[ <img src="{{{rel_path}}}images/haxe.png" target="_blank" class="small-image"/> ](http://haxe.org)   

<small class="haxedesc">Haxe is an expressive, beautiful modern programming language <br/>
      that compiles its own code into other languages. <a href="http://haxe.org/" target="_blank"> learn more</a> </small>

<br/>

---

<a name="why"></a>
### Why?

**Haxe and hxcpp give us direct, truly native access to c++.**   

Using _extern class_, we can **use native libraries from Haxe code**,   
without significant overhead, without boxing, just pure c++ -   
all while enjoying the Haxe language features as well.

### Purpose

linc is a collection of **native extern libraries** that lean on the   
hxcpp build toolchain - to create a link between native and Haxe code.   

linc libraries follow a set of guidelines to allow consistently structured,   
self contained, easy to drop in native libraries that leverage the   
powerful Haxe/hxcpp features available to us.

The purpose of linc as a collection is to build and maintain a    
repository of quality, reliable tools that are framework agnostic,   
low friction, and help expand the usefulness of Haxe in the wild.

### Ideals

A linc library aims for being :

- **agnostic** - aligns completely with the hxcpp build toolchain only
- **self contained** - focused, singular purpose, isolated libraries
- **easy to include** - only requires it's library and hxcpp as dependencies
- **Haxe friendly** - treats the user API interface as Haxe (not c++)
- **True to the c++** - follows the library patterns as closely as possible

A **linc library** conforms to **a strongly held set of guidelines**.   

[In depth details](#guidelines)

---
### IMPORTANT
**linc is a new initiative!**

We're not done tying off loose ends.    
However, many of the libraries are already usable.   

We are sharing this sooner for the sake of collaboration      
and feedback, and look forward to pushing the Haxe   
eco system forward - together with the community.

All feedback and discussions welcome,   
Please keep the discussion on [the linc repo](https://github.com/snowkit/linc) for the time being.

---

<a name="example"></a>
###Show me!

`Example.hx`

```
import sdl.SDL;

class Example {

    static var state : { window:Window, renderer:Renderer };

        //Haxe entry point
    static function main() {

        SDL.init(SDL_INIT_VIDEO | SDL_INIT_EVENTS);
        state = SDL.createWindowAndRenderer(320, 320, SDL_WINDOW_RESIZABLE);
        update();

    }

    static function update() {
        while(SDL.hasAnEvent()) {

            var e = SDL.pollEvent();
            if(e.type == SDL_QUIT) return false;

            SDL.setRenderDrawColor(state.renderer, 255,255,255,255);
            SDL.renderClear(state.renderer);
            SDL.renderPresent(state.renderer);
            SDL.delay(4);

        }
    }

}
```
`example.hxml`
```js
-main Example
-cpp cpp/
-lib linc_sdl
#-D mac or linux or windows or android or ios
```
`native haxe cpp build output`
```cpp
//Note that the output was stripped of hxcpp meta
//information for clarity with -D no-debug 
Void Example_obj::update( )
{
        bool running = true;
        while((running)){
            struct _Function_2_1{
                inline static bool Block( ){
                    {
                        SDL_PumpEvents();
                        return SDL_HasEvents((int)0,(int)65535);
                    }
                    return null();
                }
            };
            while((_Function_2_1::Block())){
                ::cpp::Struct<SDL_Event> e = linc::sdl::pollEvent();
                if (((e->type == (int)256))){
                    running = false;
                }
            }
            SDL_SetRenderDrawColor(::Example_obj::renderer,
                (int)255,(int)255,(int)255,(int)255);
            SDL_RenderClear(::Example_obj::renderer);
            SDL_RenderPresent(::Example_obj::renderer);
            SDL_Delay((int)4);
        }
    return null();
}
```
&nbsp;

---
<a name="libs"></a>
## Official Libraries

Each library has it's own readme and library specific notes in their repo.   
Be sure to check each one directly for the full picture.

[SDL](#sdl) . [OpenAL](#openal) . [Ogg](#ogg) . [ENet](#enet) . [File dialogs](#dialogs) . [stb](#stb) . [timestamp](#timestamp) . [_OpenGL_](#opengl) . [_Steam_](#steam)

<br/>

<a name="sdl"></a>
#### SDL
- SDL is a cross-platform development library designed to provide low level access to audio, keyboard, mouse, joystick, and graphics hardware
- [homepage](https://wiki.libsdl.org/)
- **status** - `WIP` - close to 100% complete
- [repo](https://github.com/snowkit/linc_sdl) 

<a name="openal"></a>
#### OpenAL
- cross-platform, 3D audio API
- [homepage](http://kcat.strangesoft.net/openal.html)
- **status** - `ready`
- [repo](https://github.com/snowkit/linc_openal) 

<a name="ogg"></a>
#### Ogg vorbis 
- Ogg vorbis file format decoding
- [homepage](https://xiph.org/ogg/)
- **status** - `ready`
- [repo](https://github.com/snowkit/linc_ogg) 

<a name="enet"></a>
#### ENet
- Networking communication library for various native platforms
- [homepage](http://enet.bespin.org/)
- **status** - `wip`
- [repo](https://github.com/snowkit/linc_enet) 

<a name="dialogs"></a>
#### dialogs
- Native file Open,Save and Folder dialogs for `Windows` `Mac` `Linux` 
- **status** - `ready`
- [repo](https://github.com/snowkit/linc_dialogs) 

<a name="stb"></a>
#### stb
- A collection of simple to use libraries for games and apps
- [homepage](https://github.org/nothings/stb)
- **status** - `in progress`
    - `ready` stb_image
    - `ready` stb_image_write
- [repo](https://github.com/snowkit/linc_stb) 

<a name="timestamp"></a>
#### timestamp
- A high resolution timer for `Mac` `Windows` `Linux` `Android` `iOS`
- **status** - `ready`
- [repo](https://github.com/snowkit/linc_timestamp) 

---

Libraries that are nearing completion:

<a name="opengl"></a>
#### OpenGL
- cross-platform, 2D and 3D rendering API
- [homepage](https://www.opengl.org/)
- **status** - `soon`
- ~~repo~~

<a name="steam"></a>
#### Steamworks
- Steam platform SDK
- [homepage](https://partner.steamgames.com/home)
- **status** - `soon`
- ~~repo~~

---

<a name="libs"></a>
## Libraries in development 
 from the community

**[Submit your library!](#contribute)**

- linc_luajit - Lua JIT scripting language [repo](https://github.com/RudenkoArt/linc_luajit)
- linc_box2d - Box2D physics library [repo](https://github.com/DanielUranga/box2d-linc)
- linc_squirrel - Squirrel scripting language [repo](https://github.com/RudenkoArt/linc_squirrel)
- linc_rtmidi - RtMidi MIDI IO library [repo](https://github.com/KeyMaster-/linc_rtmidi) 

---

<a name="guidelines"></a>
## Guidelines

Below is some (rough/WIP) information regarding   
the structure of a linc library, and how they    
would be considered for inclusion in the official list*.

These are mostly _guidelines_ and not _rules_ because c++    
is complex, and usually very nuanced, so the actual    
outcome for a single library will be based on the native    
library in question, and handled on case by case basis.

**These guidelines will be refined in practice.**

A linc library must be:  
- consistent in structure
    + files and folders consistently named 
    + folder structure matching a fixed structure
    + consistent use of c++ namespaces
- singular focus
    + ideally singular library
    + ideally singular dependency
- avoid composition of dependency
    + unless necessary
        + i.e ogg,theora,vorbis
- light weight
    + no monolith dependencies
        + e.g using qt for simple dialogs
- true to the underlying native lib
    + ideally, native documentation is close enough that it matches very closely and all existing documentation, examples, and reference can be leveraged.
- true to haxe users
    + includes workflow, types, return values
- as complete as possible
    + at minimum strive to be 100% complete
- ideally uses [native-toolkit](https://github.com/native-toolkit/) hxcpp-based submodules
    + unless drop in/header only


A more complete breakdown of all the guidelines in the xml and Haxe files   
is to come.

**Empty linc template**

You can find a starting point for a library here.
https://github.com/snowkit/linc_empty

You can replace the word "empty" in the project with the lib name and get started quicker.

**Example anatomy**

This is an example of the anatomy of a linc library.

![example anatomy]({{{rel_path}}}images/example-anatomy.png)


---

<a name="contribute"></a>
## Contribute

Want to list a library above?

- Libraries are welcome!
    + But must follow the above format and guidelines
    + Don't be discouraged, we will help out
    + High standards encourage good work
- Open a discussion on the [linc repo](https://github.com/snowkit/linc/issues)
    + This allows others to help out!
    + Allows early feedback and direction
    + Allows avoiding redundant work
- The linc team will advise and review
    + A variety of linc devs will weigh in
- If everything looks good, it's there!
---

<small>
*The official list is nothing more than an affordance   
for the end user - to know that what they are getting is    
going to be well structured, usable, and what is said on the box.
</small>

---

<a name="guide"></a>
## Native extern tips

How do you go about making an extern library?

**note** that more concrete examples can be found in each repo.   
More explicit guides (string conversion, haxe.io.Bytes etc) will be   
added here in the near future.

For even more information see the hxcpp talks:
- http://gamehaxe.com/wwx/wwx2014.swf
- http://gamehaxe.com/wwx/wwx2015.swf

Feel free to ask questions at :
- [hxcpp repo](https://github.com/haxefoundation/hxcpp)
- [haxe mailing list](https://groups.google.com/forum/#!forum/haxelang)
- [linc repo](https://github.com/snowkit/linc/issues)

**Tips**

- Define an `extern class`
    - can use `@:native('some::NativeClass')`
    - extern class has default public fields
    - define members and functions without body
        - `@:native('cpp_function')` for members
    - can use `inline` haxe functions
        - `inline function ok() { haxe code }`
- Make sure that your external code definition can be found
    - use `@:include('header.h')` on the `extern class`
    - every Haxe class that will `import`, gets the include
    - can use `@:include('./relative.h')`
        - relative to the hx file
- Use `cpp.Pointer`, `cpp.Struct`, `cpp.Reference`
    - Contextual, use the best option for the haxe code
- Normal c++ rules apply
    + Duplicate / unresolved symbols
    + Include paths, linking paths
    + Be wary of `@:include` and duplicate symbols
        * `import` the same include twice can mean duplicates

**examples**      

`vec.h`

```cpp
namespace m {
    struct vec {
        float x;
        float y;

        void scale_by(float scalar) {
            x *= scalar;
            y *= scalar;
        }
    };
}
```

`Vec.hx`

```haxe
package m;

@:include("./v.h")
@:native('m::vec')
extern class Vec {
    var x:Float;
    var y:Float;
        
    @:native('scale_by')
    function scale(scale:Float):Void;

    inline function square() : Void {
        x *= x;
        y *= y;
    }
};
```

`folders`

```
├── m/
    ├── Vec.hx
    └── vec.h
```



&nbsp;

&nbsp;

&nbsp;

---

**linc is a [snõwkit](http://snowkit.org/) community initiative**

<a href="http://snowkit.org" ><img src="{{{rel_path}}}images/snowkit.png" /></a>

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
