# Utils

This is a small set of tools that scratch various personal itches. You
may or may not find and of them remotely useful.


(See also the excellent [moreutils][].)

[moreutils]: https://joeyh.name/code/moreutils/

## dl

`find`, `mdfind`, `grep -l`, and similar commands emit a succession 
of file paths, one per line. These are very easy for programs to
parse. Unfortunately, they're a little harder for humans to read,
especially when the question the humans are trying to answer is "Which
directory should I go poking around in first?"

`dl takes a succession of paths as input, performs some grouping, and
emits a set of per-directory listings.

As a simple example, `grep -rl ruby` in one of my personal directories
generates the following output:

    ./artifacts/closest_polynomial
    ./artifacts/envy
    ./artifacts/fizzbuzz.rb
    ./artifacts/rcd
    ./artifacts/uri_descape
    ./util/dl
    ./util/jj
    ./util/loo
    ./util/path
    ./wip/frag
    ./wip/modulist
    ./wip/nn

Piped through `dl`, this produces:

    ./artifacts
      closest_polynomial
      envy
      fizzbuzz.rb
      rcd
      uri_descape

    ./util
      dl
      jj
      loo
      path

    ./wip
      frag
      modulist
      nn

The optional `-d` argument goes one step further and omits mention of
the files, listing only the directories.

## git-purge

This should really be fleshed out into a script that does some basic
argument-checking. (In particular, if not passed a path, it will
neither complain nor do anything of interest.)

It is nevertheless useful if you want to brutally and thoroughly clear
a repository, or part thereof, of any local changes. *Nota bene* that
it will **lose data**---indeed, that's the whole point---so use with
care.


## jj

This tool pretty-prints JSON. In this, it is much like the doubtless
orders-of-magnitude faster binary [jsonpp][]. Unlike jsonpp, however,
it does not expect/demand to be given input in the form of one object
per unbroken line. (To put it another way: jsonpp chokes when fed
properly- or even partially-formatted JSON, such as its own output,
while jj does not.)

There are some minor differences in output style between jj and
jsonpp. The former, for instance, always splits an array over multiple
lines, even when the array in question is empty, whereas the latter
prints empty arrays on a single line.

[jsonpp]: https://github.com/jmhodges/jsonpp

## loo

Short for **l**ast **o**ccurrence **o**nly. (Get your mind out of the
gutter.) `loo` removes all but the last instance of any repeatedly-
occurring lines from its input. Unlike `uniq`, it's not limited to
repitions of successive lines.

Given the input:

    Alice
    Bob
    Alice
    David
    Charles
    David

`loo` would emit:

    Bob
    Alice
    Charles
    David

Since it would drop the first occurrences of 'Alice' and 'David'. If invoked as `foo` (via the magic of soft/hard links), it will instead produce output containing only the *first* instance of repeatedly-occurring lines. The above input would become:

    Alice
    Bob
    David
    Charles

## path

At least 50 cents of solution to a nickel's worth of problem. So it goes. I hate deciphering the output of `echo $PATH`; now I don't have to. By default, this script pretty-prints the value of `$PATH`, with one entry per line. However, it can also be used to generate a new colon-separated value for `$PATH`, optionally removing duplicate occurrences of paths.
