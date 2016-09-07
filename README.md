## Algebird [![Build status](https://img.shields.io/travis/twitter/algebird/develop.svg)](http://travis-ci.org/twitter/algebird)
[![Codecov branch](https://img.shields.io/codecov/c/github/twitter/algebird/develop.svg?maxAge=2592000)](https://codecov.io/github/twitter/algebird)


Abstract algebra for Scala. This code is targeted at building aggregation systems (via [Scalding](https://github.com/twitter/scalding) or [Apache Storm](http://storm.apache.org/)). It was originally developed as part of Scalding's Matrix API, where Matrices had values which are elements of Monoids, Groups, or Rings. Subsequently, it was clear that the code had broader application within Scalding and on other projects within Twitter.

See the [current API documentation](http://twitter.github.com/algebird) for more information.

## What can you do with this code?

```scala
> ./sbt algebird-core/console

Welcome to Scala version 2.10.5 (Java HotSpot(TM) 64-Bit Server VM, Java 1.7.0_40).
Type in expressions to have them evaluated.
Type :help for more information.

scala> import com.twitter.algebird._
import com.twitter.algebird._

scala> import com.twitter.algebird.Operators._
import com.twitter.algebird.Operators._

scala> Map(1 -> Max(2)) + Map(1 -> Max(3)) + Map(2 -> Max(4))
res0: scala.collection.immutable.Map[Int,com.twitter.algebird.Max[Int]] = Map(2 -> Max(4), 1 -> Max(3))
```
In the above, the class Max[T] signifies that the + operator should actually be max (this is
accomplished by providing an implicit instance of a typeclass for Max that handles +).

* Model a wide class of "reductions" as a sum on some iterator of a particular value type.
For example, average, moving average, max/min, set
  union, approximate set size (in much less memory with HyperLogLog), approximate item counting
  (using CountMinSketch).
* All of these combine naturally in tuples, vectors, maps, options and more standard scala classes.
* Implementations of Monoids for interesting approximation algorithms, such as Bloom filter,
  HyperLogLog and CountMinSketch. These allow you to think of these sophisticated operations like
  you might numbers, and add them up in hadoop or online to produce powerful statistics and
  analytics.

## Documentation

To learn more and find links to tutorials and information around the web, check out the [Algebird Wiki](https://github.com/twitter/algebird/wiki).

The latest ScalaDocs are hosted on Algebird's [Github Project Page](http://twitter.github.io/algebird).

## Get Involved + Code of Conduct
Pull requests and bug reports are always welcome!

Discussion occurs primarily on the [Algebird mailing list](https://groups.google.com/forum/#!forum/algebird). 
Issues should be reported on the [GitHub issue tracker](https://github.com/twitter/algebird/issues).

We use a lightweight form of project governence inspired by the one used by Apache projects.
Please see [Contributing and Committership](https://github.com/twitter/analytics-infra-governance#contributing-and-committership) for our code of conduct and our pull request review process.
The TL;DR is send us a pull request, iterate on the feedback + discussion, and get a +1 from a [Committer](COMMITTERS.md) in order to get your PR accepted.

The current list of active committers (who can +1 a pull request) can be found here: [Committers](COMMITTERS.md)

A list of contributors to the project can be found here: [Contributors](https://github.com/twitter/algebird/graphs/contributors)

## Maven

Algebird modules are available on maven central. The current groupid and version for all modules is, respectively, `"com.twitter"` and  `0.11.0`.

Current published artifacts are

* `algebird-core_2.11`
* `algebird-core_2.10`
* `algebird-test_2.11`
* `algebird-test_2.10`
* `algebird-util_2.11`
* `algebird-util_2.10`
* `algebird-bijection_2.11`
* `algebird-bijection_2.10`

The suffix denotes the scala version.

## Questions
> Why not use spire?

We didn't know about it when we started this code, but it seems like we're more focused on
large scale analytics.

> Why not use Scalaz's [Monoid](http://docs.typelevel.org/api/scalaz/stable/7.0.4/doc/#scalaz.Monoid) trait?

The answer is a mix of the following:
* The trait itself is tiny, we just need zero and plus, it is the implementations for all the types that are important. We wrote a code generator to derive instances for all the tuples, and by hand wrote monoids for List, Set, Option, Map, and several other objects used for counting (DecayedValue for exponential decay, AveragedValue for averaging, HyperLogLog for approximate cardinality counting). It's the instances that are useful in scalding and elsewhere.
* We needed this to work in scala 2.8, and it appeared that Scalaz 7 didn't support 2.8. We've since moved to 2.9, though.
* We also needed Ring and Field, and those are not (as of the writing of the code) in Scalaz.
* If you want to interop, it is trivial to define implicit conversions to and from Scalaz Monoid.

## Authors

* Oscar Boykin <http://twitter.com/posco>
* Avi Bryant <http://twitter.com/avibryant>
* Edwin Chen <http://twitter.com/echen>
* ellchow <http://github.com/ellchow>
* Mike Gagnon <https://twitter.com/gmike>
* Moses Nakamura <https://twitter.com/mnnakamura>
* Steven Noble <http://twitter.com/snoble>
* Sam Ritchie <http://twitter.com/sritchie>
* Ashutosh Singhal <http://twitter.com/daashu>
* Argyris Zymnis <http://twitter.com/argyris>

## License
Copyright 2015 Twitter, Inc.

Licensed under the Apache License, Version 2.0: http://www.apache.org/licenses/LICENSE-2.0
