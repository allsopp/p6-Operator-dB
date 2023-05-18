# Operator::dB

[![test](https://github.com/allsopp/p6-operator-db/actions/workflows/test.yml/badge.svg)](https://github.com/allsopp/p6-operator-db/actions/workflows/test.yml)

Operator to support decibel (dB) arithmetic.

    use Operator::dB;

    put 100 + 3dB;
    # 199.52623149688796

    put 100 - 3dB;
    # 50.11872336272723

    put 10dB + 20dB;
    # 20.413927dB

## Description

The interface tries to be intuitive while avoiding ambiguity.  For example, the
following makes sense (adding 3dB is approximately equivalent to doubling).

    10 + 3dB
    # 19.952623149688794

But the following doesn't make sense. It could represent either `13dB` or
`10.8dB` (i.e. `3dB + 10dB`).

    3dB + 10 # DOESN'T WORK!

All supported operations are discussed in the following subsections.

### Addition and subtraction on numbers

Adding or subtracting decibel values to and from numbers (of `Numeric` type)
scales the number by the corresponding decibel gain:

    put 100 + 3dB;
    # 199.52623149688796

    put 100 - 3dB;
    # 50.11872336272723

### Addition and subtraction on decibels

Decibels can be added to, or subtracted from, each other.

This type of operation returns an `Operator::dB::Decibel` wrapper object:

    my $foo = 3dB + 2dB - 1dB;
    # Operator::dB::Decibel.new(x => 10, y => 0.365...)

You can get the decibel value itself with `.dB`:

    $foo.dB;
    # 3.6571819272302735

Or by stringification:

    "The gain is: $foo";
    # The gain is: 3.657182dB

Or by defining your own format with `.fmt`:

    $foo.fmt("%.1f dB(A)");
    # 3.7 dB(A)

## Caveats

This package exports overloads to built-in operators, which is potentially
reckless. But the operator signatures all contain at least one
`Operator::dB::Decibel` object (which is not built-in), so it _should_ be
fine!

The `Num` method is not implemented on the wrapper class, so many built-in
numerical operations don't work, e.g. `1dB * 1`. This is a necessary
limitation because decibel arithmetic is only semantically valid for addition
and subtraction AFAIK.

## See also

- [Operators in Raku](https://docs.raku.org/language/operators)
- [Creating operators in Raku](https://docs.raku.org/language/optut)

