---
title: "ZIO JSON Support"
---

# ZIO JSON Support

This module provides refined types Encoder/Decoder instances for [ZIO JSON](https://zio.github.io/zio-json/).

## Dependency

SBT: 

```scala
libraryDependencies += "io.github.iltotore" %% "iron-zio-json" % "version"
```

Mill:

```scala
ivy"io.github.iltotore::iron-zio-json:version"
```

## Encoder/Decoder instances

Given Encoder/Decoder for Iron enables using refined types for JSON serialization/deserialization:

```scala
import io.github.iltotore.iron.*
import io.github.iltotore.iron.constraint.all.*
import io.github.iltotore.iron.zioJson.given

import zio.json.*

case class User(name: String :| Alphanumeric, age: Int :| Greater[0])

given JsonCodec[User] = DeriveJsonCodec.gen

//Encoding
User("Iltotore", 8).toJson //{"name":"Iltotore", "age":18}

//Decoding
"""{"name":"Iltotore","age":18}""".fromJson[User] //Right(User(Iltotore, 18))
"""{"name":"Iltotore","age":-18}""".fromJson[User] //Left(.age(Should be greater than 0))
```
