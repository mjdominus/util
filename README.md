Documentation may or may not be forthcoming for these.  Some do have a
usage message though.  Most are short enough that you can easily read the
source code to see what they do.

Also relevant: [git-util](https://github.com/mjdominus/git-util) [pbmutil](https://github.com/mjdominus/pbmutil)

`addup` is a filter that adds up a series of numbers and prints the
total.  `addup` _n_ splits each line into fields and totals the n'th
field in each line.

`also` is a wrapper around `guessfile` and `menupick`, described
below.  It guesses which files you might have meant, presents a menu
to let you select which ones you actually meant, and then opens emacs
buffers for those files.

`copy-if-changed` doesn't work yet. Don't use it.

`count` generates a report of the unique lines in the input, sorted by
frequency of occurrence.

`date-arith` takes an argument list like `2014-12-23 +12`, interprets
`+12` as “plus 12 days”, and prints the resulting date in the same
format.

`dus` is like the standard `du` program, but lists the sizes of files
as well as the totals for each directory.  `dus -n` sorts the output
by size.

`every 15 command args...` runs the command every 15 seconds. Normally
each run starts 15 seconds after the last one ended; `every -s` starts
each commend 15 seconds after the last one *started*.  `-v` is
verbose; `-x` tells it to exit if any of the runs fail.

`f` is the most useful of the bunch.  `f 6` is the same as `awk
'{print $6}'`, except with 80% less typing.  `f -1` works.

`files` is a stupid one-liner: ` "${@:-.}" \( -name .git -prune \) -o -type f -print`.

`getstore _url_ _file_` fetches the file at the given URL and stores
it in the specified file.  If you omit the filename, it will be
inferred from the URL.

`guessfile` takes  a list of patterns
and prints out name of  files matching one of the patterns.
Patterns are shell glob style,
with some modifications: if the pattern contains `::`, this is
translated to a slash (for searching for Perl modules) and patterns
are _unanchored_, except that if one contains a suffix like `.txt` it
is anchored to the end.  Only the base filename is matched against the
pattern, unless the pattern contains a slash, in which case the full
path is matched.  Environment variables control the directories to be
searched (default `.` unless overridden by the `-d` flag) and to be
ignored (default `.git`).  `-i` makes the match case-insensitive.  The
`withsome` command, inspired by [Ronald Schmidt's command of the
same name](http://software-path.com/withsome.html), is a wrapper
around `guessfile` and `menupick`: `withsome command pat` guesses
files matching `pat`, filters the guesses with `menupick`, and then
runs the `command` on the selected files.

`ifchanged` watches a file using the Linux "inotify" facility and runs
a command whenever the file changes.

With arguments, `localtime` converts the arguments from epoch time to
local time and prints the conversions one per line.  Without
arguments, it is a filter, replacing an epoch time at the beginning of
any input line to the corresponding local time.

`menupick` is a filter reads a list of items from stdin, prints a menu
of the items on the terminal, repeatedly prompts the terminal for a
selection of items, and prints the selected items on stdout when the
prompting is over.  For example: `emacs $(ls | menupick)`.

    Responses to the prompt are a series of numbers, which identify
    items that are added to the current selection, and numbers with
    prefixed `!` marks, which are removed from the current selection.
    Any number of such items can be entered at once, sepearated by
    whitespace.

    An empty line, or a line that ends with `!`, terminates the
    prompting.  If an input line ends with `?`, the list of
    currently-selected items will be printed before the next prompt;
    if it ends with `??` the menu will be redisplayed, with selected
    items marked.

    If the number of items is large, lines containing `j` or `n` move
    to the next page of the menu, and `k` or `p` move to the previous
    page.

`pgrep` is an implementation of `grep` in Perl that uses Perl regular
expressions, implements a couple of features I use a lot, and
eliminates a vast number of features I never use.

`psgrep` runs `ps` and greps the output, but leaves intact the header
line that explains what the columns mean.

`runN` runs a command repeatedly with different arguments, by default
on process at a time, but the `-n N` option tells it to run up to _N_
jobs in parallel.  [This version, by Aaron
Crane](http://aaroncrane.co.uk/2008/07/runN/), has a better interface
than [my original
implementation](http://blog.plover.com/prog/runN.html).

`sort-natural` sorts its input lines like `sort`, but arranges to sort
the numeric parts of the lines numerically. For example, 

        a a1 a10 a100 a10x a2 a21 a3 ab b

sorts into the order

        a a1 a2 a3 a10 a10x a21 a100 ab b

with the numeric parts of the `a` sequence in numerical order.

`stouch` adjusts the last-modified dates on a set of files so that
they appear to have been written in the order given on the command
line.  This is useful for dealing with stupid photo gallery programs
that insist on displaying files in chronological order.

`with-memory-limit` runs a command with its memory limited by the
`rlimit` facility.

`z command args...` tries to run the command on the specified
arguments, but if any of those arguments appear to be compressed
files, it creates a pipe, runs `gzip -cd` to decompress the file into
the pipe, and attaches the command to the pipe instead.  So for
example

        perl -lne 'print if /octopus/' *

doesn't work if some of the files in the current directory are
compressed, but

        z perl -lne 'print if /octopus/' *

does.



