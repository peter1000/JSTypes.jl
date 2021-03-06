JSTypes.jl
==========

# NOTICE

**This package is unmaintained. Its reliability is not guaranteed.**

# Introduction

Mimic Javascript objects using Julian types

Often, one needs to write Julia code that interacts with Javascript. Javascript's approximation of a type system essentially places the fields of a struct inside of a hash. Instead of explicitly representing `NULL` values, the hash will often have no entry for a given key.

One can imitate this approach in Julia using Dict's, but this means that the Julia type system cannot check the sanity of the objects one creates. In contrast, using full Julia types allows Julia's compiler to do a lot of checking for the programmer. In particular, this helps when a Javascript type is defined as a container for other Javascript types, because Julia's type system will handle the recursive sanity checks without any additional work on the programmer's part.

# Usage Example

To specify a type we need two things:

* A typename, like `Foo`
* A specification of the fields of the type as a vector of tuples. Each entry looks like:
	* `fieldname`, which is a `Symbol`
	* `fieldtype`, which is a `Type`
	* `defaultval`, which is a value of type `T == fieldtype`
	* `nullable`, which is a `Bool` that determines whether the field can be set to `nothing`

Here is a specify example:

	using JSTypes
	typename = Foo
	spec = [(:field1, String, "x", false), (:field2, Int, 1, true)]
	eval(maketype(typename, spec))
	eval(makekwfunc(typename, spec))
	eval(maketojs(typename, spec))
	eval(makecopy(typename, spec))
