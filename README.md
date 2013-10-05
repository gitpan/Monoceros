# NAME

Monoceros - PSGI/Plack server with event driven connection manager, preforking workers

# SYNOPSIS

    % plackup -s Monoceros --max-keepalive-reqs=10000 --max-workers=2 -a app.psgi

# DESCRIPTION

Monoceros is PSGI/Plack server supports HTTP/1.1. Monoceros has a event-driven 
connection manager and preforking workers. Monoceros can keep large amount of 
connection at minimal processes.

                                                          +--------+
                                                      +---+ worker |
          TCP       +---------+   UNIX DOMAIN SOCKET  |   +--------+
    --------------- | manager | ----------------------+ 
                    +---------+                       |   +--------+
    <- keepalive ->              <-- passing fds -->  `---+ worker |
                                                          +--------+

Features of Monoceros

\- a manager process based on [AnyEvent](http://search.cpan.org/perldoc?AnyEvent) keeps over C10K connections

\- uses [IO::FDPass](http://search.cpan.org/perldoc?IO::FDPass) for passing a file descriptor to workers

\- supports HTTP/1.1 and also supports HTTP/1.0 keepalive

And this server inherit [Starlet](http://search.cpan.org/perldoc?Starlet). Monoceros supports following features too.

\- prefork and graceful shutdown using [Parallel::Prefork](http://search.cpan.org/perldoc?Parallel::Prefork)

\- hot deploy using [Server::Starter](http://search.cpan.org/perldoc?Server::Starter)

\- fast HTTP processing using [HTTP::Parser::XS](http://search.cpan.org/perldoc?HTTP::Parser::XS) (optional)

\- accept4(2) using [Linux::Socket::Accept4](http://search.cpan.org/perldoc?Linux::Socket::Accept4) (optional)

Currently, Monoceros does not support spawn-interval and max-keepalive-reqs.

# COMMAND LINE OPTIONS

In addition to the options supported by [plackup](http://search.cpan.org/perldoc?plackup), Monoceros accepts following options(s).
Note, the default value of several options is different from Starlet.

## \--max-workers=\#

number of worker processes (default: 5)

## \--timeout=\#

seconds until timeout (default: 300)

## \--keepalive-timeout=\#

timeout for persistent connections (default: 10)

## \--max-reqs-per-child=\#

max. number of requests to be handled before a worker process exits (default: 100)

## \--min-reqs-per-child=\#

if set, randomizes the number of requests handled by a single worker process between the value and that supplied by `--max-reqs-per-chlid` (default: none)

## \--max-keepalive-connection=\#

max, number of connections to keep in the manager process. If you want to increase this value, You should check your system limitations. (default: half number of POSIX::\_SC\_OPEN\_MAX)

## \--max-readahead-reqs=\#

max. number of requests to continue to read a request in a worker process. Monoceros can read a next request after the response for maximum throughput. (default: 100)

## \--min-readahead-reqs=\#

if set, randomizes the number of requests to continue to read a request between the value and that supplied by `--max-readahead-reqs` (default: none)



# RECOMMENDED MODULES

For more performance. I recommends you to install these module.

\- [EV](http://search.cpan.org/perldoc?EV)

\- [HTTP::Parser::XS](http://search.cpan.org/perldoc?HTTP::Parser::XS)

# SEE ALSO

[Starlet](http://search.cpan.org/perldoc?Starlet), [Server::Starter](http://search.cpan.org/perldoc?Server::Starter), [AnyEvent](http://search.cpan.org/perldoc?AnyEvent), [IO::FDPass](http://search.cpan.org/perldoc?IO::FDPass)

# LICENSE      

Copyright (C) Masahiro Nagano

This library is free software; you can redistribute it and/or modify
it under the same terms as Perl itself.

# AUTHOR

Masahiro Nagano <kazeburo@gmail.com>
