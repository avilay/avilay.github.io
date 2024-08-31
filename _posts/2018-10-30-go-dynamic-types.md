---
layout: post
title: Go Dynamic Types
date: '2018-10-30 07:00:00'
tags:
- from-medium
---

### Go is a statically typed language, where do dynamic types come in?

I’ll attempt to answer this question by building a mental model of Go’s type system which in turn will help us reason about its program structure. Warning: this blog is the result of speculation based on empirical data. It is _not based on any internal knowledge of Go_. If any Go experts are reading this, please feel free to comment and correct.

## Static Types (recap)

First lets set the context for the rest of the discussion by doing a quick recap of static types.

    type AudioPlayer struct {
        Volume int
    }
    ap := AudioPlayer{Volume: 10}

The variable `ap` has **two** characteristics:

- Its **type** is `AudioPlayer`
- Its **value** is the `AudioPlayer` object

## Interfaces

Interfaces introduce the concept of dynamic types in Go.

    type Player interface {
        Play(content string)
    }
    func (ap AudioPlayer) Play(content string) {
        fmt.Printf("Playing audio %v at volume %d\n", content, ap.Volume)
    }
    ap := AudioPlayer{Volume: 10}
    var p Player = ap

The variable `p` has **three** characteristics (as opposed to two characteristics of `ap`):

- Its **_type_** is `Player`.
- Its **_value_** is some sort of pointer to `ap`, much like vtable pointers in C.
- Its **_dynamic type_** is the same as the type its value is pointing to, which is `AudioPlayer`.

Reflecting on `p` will always give its dynamic type, i.e., `fmt.Println("%T", p)` will print `AudioPlayer`. And calling `p.Play("hello world")` will call `AudioPlayer`'s `Play`method. In other words, the receiver of `Play` will be the dynamic type of `p`. To clarify this point further, if there was another struct that had also implemented the `Player`interface, and at runtime Go encountered a `Player` variable calling `Play`, how will it know which `Play` to invoke?

    type VideoPlayer struct {
        Brightness int
    }
    func (vp VideoPlayer) Play(content string) {
        fmt.Printf("Playing video %v at brightness %d\n", content, vp.Brightness)
    }
    func doSomething(p Player) {
        p.Play("which Play will be invoked?")
    }

Go will look at the dynamic type of `p` which might get set at runtime, and invoke the appropriate method.

    func main() {
        var p Player
        if os.Args[1] == "video" {
            p = VideoPlayer{Brightness: 10}    
        } else if os.Args[1] == "audio" {
            p = AudioPlayer{Volume: 10}
        }
        doSomething(p)
    }

This code will result in Go calling the `Play` method depending on user input.

## Dynamic Dispatch

For static types like structs, Go already knows at compile time whether or not a method is implemented by it. But dynamic types, a.k.a interfaces, can have their values set to any compatible struct (or any other type) at runtime. Like in the example above, `p` can be assigned to either a `VideoPlayer` or an `AudioPlayer`. Go does not have this information at compile time. This is why when a method is invoked, instead of looking for it in the value of `p` itself, the Go runtime follows the pointer in `p`'s value to land on the actual object that will have the method implemented. This seems similar to how virtual functions are implemented in C++.

## Type Assertions

If we need to figure out the dynamic type of an interface at runtime, we can use type assertions.

    func doSomething(p Player) {
        if paudio, ok := p.(AudioPlayer); ok {
            paudio.Volume = 20
            paudio.Play("play music")
        } else if pvideo, ok := p.(VideoPlayer); ok {
            pvideo.Brightness = 20
            pvideo.Play("watch video")
        }
    }

Go uses special syntax for that which looks like `iface.(ConcreteType)`. For a detailed explanation of type assertions see the relevant [language spec page](https://golang.org/ref/spec#Type_assertions). In the above code, the type of `paudio` will be `AudioPlayer` regardless of whether the dynamic type of `p` is `AudioPlayer` or not. If it is not, then `ok` will be `false` and the value of `paudio` will be `nil`. In fact, the above scenario is so common that Go has (yet another) special syntax for this called [type switching](https://golang.org/doc/effective_go.html#type_switch).

