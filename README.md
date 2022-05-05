# Julia Programming Language

First off, if you are wondering about the programming language's name **Julia** like I was, apparently [one of the developers just thought that the name was pretty](https://www.infoworld.com/article/2616709/new-julia-language-seeks-to-be-the-c-for-scientists.html). Thats it

*Post Formatting Notes: In the post, I just bolded **Julia** when in reference to the programming language **Julia**. If a reader is named Julia, I am sorry but this is not a post about you* :(

## Brief History

**Julia** was developed by several people but was designed by 3 core developers.
Developers include, [Jeff Bezanson, Stefan Karpinski, Viral B. Shah,](https://github.com/JuliaLang/julia/blob/master/LICENSE.md) [and other github contributors](https://github.com/JuliaLang/julia/graphs/contributors).
The language [first appeared in 2012](https://julialang.org/blog/2012/02/why-we-created-julia/) where it's "debut" was posted in a blog as a "1.0 release post" along with comments about its conception.

### Intended Use
**Julia** was made because the developers wanted a language that was as fast as the C language while also having simple syntax which would allow for flexibility for the programmers, and the devs also wanted a language that would be "easy for statistics as R [is]", have natural "string processing as Perl [does]", something "powerful for linear algebra as Matlab", and program glue to link all these systems together. There was also a desire for an open source license that has a generally open license for public use. Overall, the developers desired for the intended audience to be programmers that come from different language backgrounds with the intended usage of doing better than those languages (C, Matlab, Lisp, Python, Ruby, R, Perl, Mathematica, etc)
[(source)](https://julialang.org/blog/2012/02/why-we-created-julia/).

### Runtime Tools

**Julia** is very language flexible as it allows for "interoperability" with other C based languages using the `ccall` keyword which allows for calling shared library functions. There are also packages for calling other languages (.NET languages, JVM languages, **Julia** itself, and even hardware VHDL). It is also possible to use **Julia** in other languages using external packages [(source)](https://syl1.gitbook.io/julia-language-a-concise-tutorial/language-core/interfacing-julia-with-other-languages).

The language was implemented with [C, C++, Scheme, and other smaller dependencies](https://github.com/JuliaLang/julia) while the **Julia** base code and the core libraries were made with **Julia** itself.




## Julia's Characteristics
- **Julia** is a "multi-paradigm" language. Its primary distinctive characteristic is its ["multiple dispatch" feature](https://julialang.org/) where the language chooses which function to use on the fly based on the run-time/dynamic typing of its attributes (Lisp also uses multiple-dispatch though at a lesser extent). **Julia** is also procedural language where it follows instructions in order (Java also follows this feature). [Here is a good read](https://towardsdatascience.com/the-depressing-challenges-facing-the-julia-programming-language-in-2021-34c748968ab7) that delves a little deeper about **Julia**'s features / multiple dispatch paradigm.
- **Julia** uses a "Just-in-time" compiler that converts high-level code into machine code during run-time before running the correlated code. For some quirky reason, the community likes to call it ["Just-ahead-of-time"](https://web.archive.org/web/20200613164839/https://juliacomputing.com/blog/2019/02/19/growing-a-compiler.html).
- [Dynamic, Strong](https://erik-engheim.medium.com/dynamically-typed-languages-are-not-what-you-think-ac8d1392b803), Explicit (nominative), impure (assumption basis: side-effects such as printing are allowed) are all ways to describe the typing of the **Julia** language. It also follows strict evaluation (eager evaluation) where a functions arguments are evaluated completely before applying the function (citation needed).
- **Julia** follows a static (lexical) scoping. Certain constructs offer different scope types however. Example: `modules` are given a global scope whereas `structs` and `loops` are given local scopes.

There is also subtle difference in local scope constructs where hard local scopes are simply given its assignment in its scope (and thus can be nested without restriction) and soft local scopes give assignment depending if there is already a global assignment with the same name. In soft local scopes, in a non-interactive context, the local variable is simply created while also outputting a warning stating the ambiguity of the variable. This contrasts to interactive contexts, where the global variable is instead assigned rather than assigning the local variable.

Within both hard and soft local scopes, some can be nested while others are only allowed globally. Example: `module`'s and `struct`'s are only allowed to be defined globally whereas `loop`'s and `do` `let` blocks are allowed globally and locally.
[(source)](https://docs.julialang.org/en/v1/manual/variables-and-scoping/)
- **Julia**'s base and standard libraries are written in **Julia** (itself) including primitive operations such as integer arithmetic [(souce)](https://docs.julialang.org/en/v1/). I was surprised when I saw how much of **Julia**'s source code was written in **Julia** itself.
- Various base mechanisms:
  - Importing `module`'s in **Julia** is as simple as calling `module module_name` (global scope). There are also different ways to specifically use certain functions/vars/fields (and names in general) of a module through the use of `export`, `using`, and `import` [Module Documentation](https://docs.julialang.org/en/v1/manual/modules/)
  - The most common type in **Julia** is the `struct` type where it is immutable by default (can define it as mutable) and acts as a container for field variables that can be referenced.
[`Struct` Documentation](https://docs.julialang.org/en/v1/base/base/#struct)
  - The `abstract type` keyword in **Julia** declares a new type that is not instantiated but rather referenced as a type ancestor for another variable. Abstract type nodes can also have a supertype of another type. (Documentation right below `struct` doc)
- Overall, **Julia** (at first glance) syntactically looks like Python and claims to be as flexible as Python while still offering speedy computations for stats calculations, data structure management, and parallel processing. There is also a predefined type for complex and rational numbers for complex mathematical work (ex. `(1 + 2im)*(2 - 3im)`). The data structure libraries in **Julia** give a similar feel to the NumPy library in Python (ex. `zeroes(2,3)` is a 2 by 3 matrix filled with zeroes). **Julia** also allows for direct C library calling without the usage of wrappers (or "glue code") which sounds pretty neat.




## Julia Code and Discussion
[Official Getting Started Documentation](https://docs.julialang.org/en/v1/manual/getting-started/)

### Hello World Implementation

In interactive mode, writing Hello World is as simple as adding quotes around the string:
```
julia> "Hello World"
"Hello World"

# Alternatively:
julia> print("Hello World")
Hello World
```
As for non-interactive mode, just write the function that we saw above into a file, then run it.
```
# Within the file
print("Hello World")
```
To run from command line, run:

`julia script_name.jl [arg1] [arg2]...`

or from the interactive mode:

`julia> include("path_to_file.jl")`

Simple stuff!


### Calling C Libraries

We can use the ccall(...) method to call on C libraries directly.
Here is an example of calling the `clock` function from C's standard library within **Julia**
```
julia> ccall(:clock, Int32, ())
111149
```
Here are the parameters for ccall():
1. The first parameter of ccall() is a `(:function, library)` pair (or just a `:function` if the function is already a symbol in the process or part of libc)
2. The second parameter is simply the return type
3. The third parameter are the input types (tuple) that the function would take
4. The fourth+ parameter[s] are the actual inputs for the function where each new input would be a new parameter

### Prime Factor Function

I wanted to try running something that would do a lot of division and array/vector adding so I chose to implement a brute force prime factors function:

```
function primeFactor(n)
    i = 2
    # create an array for the output
    x = Vector{}() # assuming an "any" type
    # can also declare it as:
    # Vector{UInt32}() as an unsigned 32 bit int
    while i^2 <= n
        if n % i != 0
            i = i + 1
        else
            n = div(n, i)
            # push is like adding x in (x:tail)
            push!(x,i)
            # whereas append is like [x] ++ tail
        end
    end
    if n > 1
        push!(x,n)
    end
    print(x)
end
```


Here is the run and the output (first function uses an untyped Vector whereas the second function uses a Float Vector):

```
julia> include("./prime factor.jl")
primeFactor (generic function with 1 method)

julia> primeFactor(123123123123)
Any[3, 7, 11, 13, 41, 101, 9901]

julia> primeFactorFloat(123123123123)
[3.0, 7.0, 11.0, 13.0, 41.0, 101.0, 9901.0]
```
Pretty neat


### More Complex Example

Here is a code snippet found on [**Julia**'s example page on their website](https://julialang.org/learning/code-examples/) that outputs something cool:

```
function mandelbrot(a)
    z = 0
    for i=1:50
        z = z^2 + a
    end
    return z
end

for y=1.0:-0.05:-1.0
    for x=-2.0:0.0315:0.5
        abs(mandelbrot(complex(x, y))) < 2 ? print("*") : print(" ")
    end
    println()
end
# Taken from: https://rosettacode.org/wiki/Mandelbrot_set#Julia
```
This program outputs a (well known) cool looking ascii fractal.
```
                                                                                
                                                           **                   
                                                         ******                 
                                                       ********                 
                                                         ******                 
                                                      ******** **   *           
                                              ***   *****************           
                                              ************************  ***     
                                              ****************************      
                                           ******************************       
                                            ******************************      
                                         ************************************   
                                *         **********************************    
                           ** ***** *     **********************************    
                           ***********   ************************************   
                         ************** ************************************    
                         ***************************************************    
                     *****************************************************      
 ***********************************************************************        
                     *****************************************************      
                         ***************************************************    
                         ************** ************************************    
                           ***********   ************************************   
                           ** ***** *     **********************************    
                                *         **********************************    
                                         ************************************   
                                            ******************************      
                                           ******************************       
                                              ****************************      
                                              ************************  ***     
                                              ***   *****************           
                                                      ******** **   *           
                                                         ******                 
                                                       ********                 
                                                         ******                 
                                                           **                   
                                                                                
```



## Final Notes

I was compelled by the developers' claims of **Julia**'s speed and flexibility and had to try it out myself. Running the programs shown above proved that the language is indeed quite fast. However, the language is not without its faults. The main issue is something I found during my research where even though **Julia** *can* be used in other languages, **Julia**'s libraries just aren't worth using as there are many other more developed, mature options to choose from.

There is also the fact that **Julia** takes up a relatively large amount of memory to run even the simplest of programs. For example, even loading my hello world program from above caused ~120MB memory usage. That's not good. Especially for memory constrained programs where every little bit of memory (pun intended) counts, **Julia** is not a good option. The program also desires to run as fast as the C language, but other programs can literally just use C if they care about performance (C also has better memory performance).

Another obvious downside is the wacky syntax that **Julia** uses. Even though it looks simple, there are some odd syntax decisions during development that I don't really agree with. For example, iterators are daunting in **Julia**. Here is [an example taken from their blog](https://julialang.org/blog/2018/07/iterators-in-julia-0.7/) (**Julia** 0.7 so a little outdated but same syntax):
```
iter_result = iterate(iterable)
while iter_result !== nothing
    (element, state) = iter_resultpython
    # body
    iter_result = iterate(iterable, state)
end
```
This is a lot of work for something that is a one liner in Python.

Overall, **Julia** is unique. It is a very young language that seems to specialize in statistical operations with its speedy data structure manipulation. You can also use [emojis as variable names](https://juliapackages.com/p/emojisymbols) which can be seen as a plus. However, in its young state, it does not have the tools to overtake other languages such as R or Python. Though it claims to have easier syntax than most languages, some things are just arbitrarily confusing. There are also many raw performance kinks that need to be worked out such as memory and latency (speed is okay though) Perhaps in a couple more years when the language develops more and matures, I can recommend the language for stats work and general data manipulation, but in its state now, it feels more like a beta language.