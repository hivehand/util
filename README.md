# Utils

This is a small set of tools that scratch various personal itches. You
may or may not find any of them remotely useful.

(See also the excellent [moreutils][].)

[moreutils]: https://joeyh.name/code/moreutils/

## alert

A thin wrapper around [blink1-tool](http://blink1.thingm.com/blink1-tool/), the
command-line interface to the nifty little USB LED gadget known as
[blink(1)](http://blink1.thingm.com).

It triggers ten flashes over five seconds, in the color of your choice. If no
color is given, it defaults to white. Specify a time in the future and it will
wait until then to flash.

## bur

Short for "brew update report". It takes the output of `brew update` and
generates an HTML document consisting of a four-column table: the three left
column contain the name of the formula, possibly accompanied by adornments, and
linked to the corresponding project's home page; each cell in the right column
contains the corresponding description. The aforementioned adornments include
green check marks denoting installed formulae, and the previous name and an
arrow in the case of renamed formulae.

## dl

`find`, `mdfind`, `grep -l`, and similar commands emit a succession 
of file paths, one per line. These are very easy for programs to
parse. Unfortunately, they're a little harder for humans to read,
especially when the question the humans are trying to answer is "Which
directory should I go poking around in first?"

`dl` takes a succession of paths as input, performs some grouping, and
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

The optional `-d` argument goes one step further and omits mention of the files,
listing only the directories.


## git-cull

GitHub's pull-request model means that the local copy of any work repository I
frequently pull from tends to accumulate lots of old branches, since the GitHub
deletion of those branches immediately following the merge has no effect upon
the corresponding local branches. `git cull` steps in to fix this. It notes the
starting branch, switches over to master, gets a list of merged branches, and
proceeds to delete them. Finally, if the starting branch wasn't one of the ones
culled, it switches back to it.


## git-purge

If you were to take a cue from Ripley in *Aliens*, and set out to build a
flamethrower that sported both a chainsaw bayonet and a grenade launcher capable
of flinging water balloons filled with hydrochloric acid, then you might
ultimately produce a tool as dangerous, as prone to creating inadvertent
collateral damage, as this one.

There are nevertheless times when you want to brutally and thoroughly clear a
repository, or part thereof, of any local changes, without going to the trouble
of cloning a fresh copy from origin. That's where this script comes in.

Given a target path, it will:

    git reset -q HEAD ${target}
    git checkout -- ${target}
    git clean -fd ${target} 

If *not* given a target path, it will default to the current directory. Just to
be crystal clear: this is a tool designed from the outset to **throw away**
data, so use with care.


## git-report

A very, *very* simple wrapper that just does the following:

    git fetch
    git status --column
    git stash list

It's a one-command mechanism for getting a quick report on the state of the
current repository and branch.


## jj

This tool pretty-prints JSON. In this, it is much like the doubtless
orders-of-magnitude faster binary [jsonpp][]. Unlike jsonpp, however,
it does not expect/demand to be given input in the form of one object
per unbroken line. (To put it another way: `jsonpp` chokes when fed
properly- or even partially-formatted JSON, such as its own output,
while `jj` does not.)

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

Since it would drop the first occurrences of 'Alice' and 'David'. If
invoked as `foo` (via the magic of soft/hard links), it will instead
produce output containing only the *first* instance of repeatedly-
occurring lines. The above input would become:

    Alice
    Bob
    David
    Charles


## modulist

Short for "module list". Stash lists of the locally-installed Homebrew packages
and Ruby gems to `~/.config/packages`. Each created file has a datestamp:
previous stashed lists for the same host, if present, are deleted after the new
one has been written.


## path

At least 50 cents of solution to a nickel's worth of problem. So it goes. I hate
deciphering the output of `echo $PATH`; now I don't have to. By default, this
script pretty-prints the value of `$PATH`, with one entry per line. However, it
can also be used to generate a new colon-separated value for `$PATH`, optionally
removing duplicate occurrences of paths.

## spine

Take output split into two columns by a known substring, the 'spine', and align
it so that it's centered on said spine.

    foo: bar
    very long string: baz

Would become:

                 foo: bar
    very long string: baz
