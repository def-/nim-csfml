nim-csfml
=========
#### Nim Bindings for [Simple and Fast Multimedia Library][sfml] (through [CSFML][]).

**See [introduction](#introduction), [examples](examples), [documentation][].**

The files <em>[src](src)/csfml\_\*\_gen.nim</em> are [automatically generated](generate) from CSFML's header files. They provide the base CSFML API. The files <em>csfml_\*.nim</em> build upon them, adding compatibility with SFML API.  
*csfml.nim* automatically imports *system*, *window* and *graphics*; *audio* should be imported separately; *network* is not implemented.

*nim-csfml* currently supports CSFML 2.1 only, but it may be possible to generate it for older versions of CSFML. It has been tested on Linux 64-bit and Windows 7 with MinGW 32-bit.  
Support for SFML 2.2 will be added immediately after CSFML 2.2 is released.

This library can be installed using <code>[nimble][] install csfml</code>.

[CSFML][] 2.1, which requires [SFML][] **2.1**, must be installed to use it.
On Windows you can just [download CSFML][csfml] and put the DLLs (which seem to be statically linked with SFML) in your project folder instead.

*nim-csfml*'s version number (`x.y.z`): `x.y` corresponds to the supported CSFML version; `z` is for the project's own point releases.


Introduction
------------

*nim-csfml* allows you to use [SFML][], which is a library made in C++. So most information and [tutorials][sfml-tutorials] for SFML revolve around C++. It is a good idea to get familiar with SFML itself first.

The API attempts to be very similar to SFML's, but some general changes are present:

- To construct an object (`sf::SomeType x(param)`), use a corresponding procedure (there are 2 variations):
    - `var x = newSomeType(param)`, which means it is a `ptr object`.
        - Such objects have [destructors][nim-destructors] associated with them (no need to call `destroy` manually).
        - Never create them using `new`.
    - `var x = someType(param)`, which means it is an `object` (in CSFML it corresponds to a simple `struct`).
        - `Vector2(i|f)`, `Vector3f` and `(Int|Float)Rect` should be created using special `vec2`, `vec3` and `rect` procs.
    - Member functions, such as `loadFromFile`, that are used for initialization, are also represented as constructor procs described above.
- Getter, setter functions are changed:
    - `x.getSomeProperty()` and `x.isSomeProperty()` both become `x.someProperty`.
    - `x.setSomeProperty(v)` becomes `x.someProperty = v`.
- Some renames of members were necessary because of reserved words:
    - `type`: `kind`, `object`: `obj`, `string`: `str`, `bind`: `bindGL`.
- `enum` names were taken from CSFML, but changed to remove their common prefix to resemble SFML's values.
    - Expect surprises in their naming. Use the [documentation][].
    - `enum`s are all `pure`: use `EnumType.Value`.
    - A few `enum`s are just represented as a list of constants, because they are not really enumerations.
    - SFML sometimes uses `enum` values as bitmasks. You can combine them using the `|` (not `or`) operator defined for `BitMaskU32` in the *util* module.
- Unicode is supported and easy to use (no need for `sf::String` or strange conversions):
    - CSFML's functions can deal with locale-dependent C strings (which should not be used)...
    - and UTF-32 sequences, which have been wrapped for convenient use with Nim's normal UTF-8 strings.
- Type differences:
    - Sadly, `cint` and `cfloat` will be present everywhere, so explicit conversions to/from Nim's normal types may be required.
    - `unsigned int` is mapped as `cint`, etc., so you don't have to bother with unsigned conversions. This shouldn't cause problems, but it might.
    - The *util* module contains some types that provide implicit conversions.
        - For example `sfBool` which is defined as `int`, is mapped to the `IntBool` type with conversions to Nim's `bool`.
- If you want to use some particular CSFML function but don't know what it maps to in *nim-csfml*, you can just search for its name in the [documentation][].
- Most of the [documentation][] is taken directly from CSFML, so don't be surprised if it talks in C/C++ terms.

See [examples](examples) to learn more.


Acknowledgements
----------------

[License](LICENSE): zlib/libpng

This library uses and is based on [SFML][] and [CSFML][].

[nimrod-sfml][] was a great source of knowledge.

[Nim][] and [Python][] programming languages are used.


[documentation]: http://blaxpirit.github.io/nim-csfml/
[sfml]: http://www.sfml-dev.org/ "Simple and Fast Multimedia Library"
[csfml]: http://www.sfml-dev.org/download/csfml/
[sfml-tutorials]: http://www.sfml-dev.org/tutorials/
[nim]: http://nim-lang.org/
[nim-destructors]: http://nim-lang.org/manual.html#destructors
[python]: http://python.org/
[nimble]: https://github.com/nim-lang/nimble
[nimrod-sfml]: https://github.com/fowlmouth/nimrod-sfml
