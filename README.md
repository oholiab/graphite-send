graphite-send
=============

What?
-----
A bash wrapper around nc with basic positional argument parsing, and support for
a basic config file and reading value from stdin.

Why?
----
Because I needed it to do a thing for some stuff in a way that does what it says
on the tin and takes a config file.

How?
----
    $ ./graphite-send -h
    Usage: ./graphite-send -c CONFIG
           ./graphite-send GRAPHITE_PORT GRAPHITE_PORT STAT_NAME [VALUE]

Pretty simple, to try it out without actually sending any metrics, clone the
repository down and in a seperate session run

    nc -l localhost 31337

Then run

    ./graphite-send -c config

Which will wait for a line of stdin before sending it to the netcat instance.
This is an example of config file mode, where the value is read in from stdin.

To run in CLI mode, you simply supply 3 positional arguments, which are the
GRAPHITE_SERVER, GRAPHITE_PORT and STAT_NAME (which must be fully qualified)
optionally followed by the VALUE for the statistic. If VALUE is not given on the
command line then the script will wait for it on STDIN.

So to test this in our example with the netcat process above:

    ./graphite-send localhost 31337 some.stuff 1234

graphite-send always adds the timestamp in itself from the system time.

Hacking
-------

Contributions welcome, but just a few things to note:

  * Other than netcat, this script has no other dependencies and is one file
    big. The former is not a hard limit, but I'd like to keep it to commonly
    found *NIX utilities. The latter is so it's easy to distribute.
  * To add arguments, you literaly just need to add them to the argument array
    at the top of the script somewhere before VALUE. That's it. VALUE needs to
    stay at the end as it's optional.

Other than that, fill your boots.
