# List::AutoNumbered - Add line numbers to lists while creating them



Quick summary of what the module does.

Perhaps a little code snippet.

    use List::AutoNumbered;

    my $foo = List::AutoNumbered->new();
    ...

# GLOBALS

## $TRACE

(Default falsy) If truthy, print trace output.  Must be accessed directly
or requested on the `use` line, e.g.:

    use List::AutoNumbered; $List::AutoNumbered::TRACE=1;

or

    use List::AutoNumbered '*TRACE'; $TRACE=1;

# METHODS

## new

Constructor.  Call as `$class->new($number)`.  Each successive element
will have the next number, unless you say otherwise (e.g., using ["LSKIP"](#lskip)).

## size

Returns the size of the array.  Like `scalar @arr`.

## last

Returns the index of the last element in the array.  Like `$#array`.

## arr

Returns a reference to the array being built.  Please do not modify this
array directly until you are done loading it.  List::AutoNumbered may not
work if you do.

## load

Push a new record with the next number on the front.  Usage:

    $instance->load(whatever args you want to push);

Or, if the current record isn't associated with the number immediately after
the previous record,

    $instance->load(LSKIP $n, args);

where `$n` is the number of lines between this `load()` call and the last one.

Returns a coderef that you can call to chain loads.  For example, this works:

    $instance->load(...)->(...)(...)(...) ... ;
    # You need an arrow ^^ here, but none after that.

## add

Add to the array being built, **without** inserting the number on the front.
Does increment the number and (TODO) respect skips, for consistency.

Returns the instance.

## LSKIP

A convenience function to create a skipper.  Prototyped as `($)` so you can
use it conveniently with ["load"](#load):

    $instance->load(LSKIP 1, whatever args...);

If you are using line numbers, the parameter to `LSKIP` should be the number
of lines above the current line and below the last ["new"](#new) or ["load"](#load) call.
For example:

    my $instance = List::AutoNumbered->new(__LINE__);
    # A line
    # Another one
    $instance->load(LSKIP 2,    # two comment lines between new() and here
                    'some data');

# PACKAGES

## List::AutoNumbered::Skipper

This package represents a skip and is created by ["LSKIP"](#lskip).
No user-serviceable parts inside.

### new

Creates a new skipper.  Parameters are for internal use only and are not
part of the public API.

# AUTHOR

Christopher White, `<cxwembedded at gmail.com>`

# BUGS

Please report any bugs or feature requests through the web interface at
[https://github.com/cxw42/List-AutoNumbered/issues](https://github.com/cxw42/List-AutoNumbered/issues).  I will be notified, and
then you'll automatically be notified of progress on your bug as I make
changes.

# SUPPORT

You can find documentation for this module with the perldoc command.

    perldoc List::AutoNumbered

You can also look for information at:

- MetaCPAN

    [https://metacpan.org/pod/List::AutoNumbered](https://metacpan.org/pod/List::AutoNumbered)

- CPAN Ratings

    [https://cpanratings.perl.org/d/List-AutoNumbered](https://cpanratings.perl.org/d/List-AutoNumbered)

# ACKNOWLEDGEMENTS

Thanks to zdim for discussion on the
[Stack Overflow question](https://stackoverflow.com/q/50510809/2877364)
that was the starting point for this module.

# LICENSE AND COPYRIGHT

Copyright 2019 Christopher White.

This program is free software; you can redistribute it and/or modify it
under the terms of either: the GNU General Public License as published
by the Free Software Foundation; or the Artistic License.

See [http://dev.perl.org/licenses/](http://dev.perl.org/licenses/) for more information.
