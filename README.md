Documentation may or may not be forthcoming for these.  Some do have a
usage message though.  Most are short enough that you can easily read the
source code to see what they do.

`addup` is a filter that adds up a series of numbers and prints the
total.  `addup `_n_ splits each line into fields and totals the _n_th
field in each line.

`copy-if-changed` doesn't work yet. Don't use it.

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

`menupick` is a filter reads a list of items from stdin, prints a menu
of the items on the terminal, repeatedly prompts the terminal for a
selection of items, and prints the selected items on stdout when the
prompting is over.  Responses to the prompt are a series of numbers,
which identify items that are added to the current selection, and
numbers with prefixed `!` marks, which are removed from the current
selection.  Any number of such items can be entered at once,
sepearated by whitespace.  An empty line, or a line that ends with
`!`, terminates the prompting.  If an input line ends with `?`, the
list of currently-selected items will be printed before the next
prompt; if it ends with `??` the menu will be redisplayed, with
selected items marked.  For example: `emacs $(ls | menupick)`.

`mkpasswd` generates and prints a random password. It has a bunch of
options I never use.

`psgrep` runs `ps` and greps the output, but leaves intact the header
line that explains what the columns mean.

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



