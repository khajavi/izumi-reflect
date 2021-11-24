[![Project stage][Stage]][Stage-Page]
![Build](https://github.com/zio/izumi-reflect/workflows/Build/badge.svg)

# izumi-reflect

> @quote: Looks a bit similar to TypeTag

`izumi-reflect` is a fast, lightweight, portable and efficient alternative for `TypeTag` from `scala-reflect`.

`izumi-reflect` is a lightweight model of Scala type system and provides a simulator of the important parts of the Scala typechecker.

## Why `izumi-reflect`

1. `izumi-reflect` compiles faster, runs a lot faster than `scala-reflect` and is fully immutable and [thread-safe](https://github.com/scala/bug/issues/10766),
2. `izumi-reflect` supports Scala.js, Scala Native,
3. **`izumi-reflect` is published for Scala 3**, you may check port status [here (#22)](https://github.com/zio/izumi-reflect/issues/22), 
4. `izumi-reflect` allows you to obtain tags for unapplied type constructors (`F[_]`) and combine them at runtime.

## Credits

`izumi-reflect` has been created by [Septimal Mind](https://7mind.io) to power [Izumi Project](https://github.com/7mind/izumi),
as a replacement for `TypeTag` in reaction to a lack of confirmed information about the future of `scala-reflect`/`TypeTag` in Scala 3 ([Motivation](https://blog.7mind.io/lightweight-reflection.html)),
and donated to ZIO.
This repository contains an independent and more conservative copy of the code comparing to Izumi one.

<p align="center">
  <a href="https://izumi.7mind.io/">
  <img width="40%" src="https://github.com/7mind/izumi/blob/develop/doc/microsite/src/main/tut/media/izumi-logo-full-purple.png?raw=true" alt="Izumi"/>
  </a>
</p>


## Limitations

`izumi-reflect` model of the Scala type system is not 100% precise, but "good enough" for the vast majority of the usecases.

Known limitations are:

1. Type boundaries support is very limited because of a [problematic behavior](https://github.com/scala/bug/issues/11673) of Scala 2.13 compiler,
2. Recursive type bounds (F-bounded types) are not preserved and may produce false positives,
3. Existential types written with `forSome` are not supported and may produce unexpected results,
4. Path-Dependent Types are based on variable names and may cause unexpected results with variables with different names but the same type or vice-versa (vs. Scala typechecker)
5. At the moment Scala 3 port does not support Path-Dependent Types, and Structural Refinements. This will be fixed in future.

## Contributing

When working on the Scala 3 version of the codebase in Intellij, we recommend importing using BSP mode instead of sbt mode, see [Dotty#Importing the Project Using BSP.](https://github.com/lampepfl/dotty/blob/b7d2a122555a6aa44cc7590852a80f12512c535e/docs/docs/usage/ide-support.md#importing-the-project-using-bsp) 

## Build

`build.sbt` is generated by [sbtgen](https://github.com/7mind/sbtgen). During development you may not want to mess with ScalaJS and ScalaNative, you may generate a pure-JVM Scala project:

```bash
./sbtgen.sc
```

Once you finished tinkering with the code you may want to generate full project and test it for all the platforms:

```bash
./sbtgen.sc --js --native
sbt +test
```

To develop using Scala 2 invoke sbtgen with a scala version argument:

```bash
./sbtgen.sc 2 // 2.13
./sbtgen.sc 2.12 // 2.12
```

Likewise with Scala 3:

```bash
./sbtgen.sc 3
```

You may also set Scala version from inside Intellij using `sbt -> sbt settings -> Open cross-compiled projects Scala 3 / Scala 2 projects as:`

[Stage]: https://img.shields.io/badge/Project%20Stage-Production%20Ready-brightgreen.svg
[Stage-Page]: https://github.com/zio/zio/wiki/Project-Stages
