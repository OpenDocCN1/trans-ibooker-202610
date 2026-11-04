One of the great features of the Swoole extension is its **coroutine support**. What this means, in real-life terms, is that we don't have to perform major surgery on applications written using the synchronous programming model. Swoole will automatically pick out blocking operations such as filesystem access and database queries and allow these operations to be placed on hold while the rest of the application proceeds. Because of this support, you can often simply run the synchronous application using Swoole, resulting in an immediate performance boost.

Another really great feature of the Swoole extension is `Swoole\Table`. This feature lets you create, entirely in memory, the equivalent of a database table that can be shared between multiple processes. There are many possible uses for such a construct, and the potential performance gain is truly staggering.

The Swoole extension has the ability to listen for **User Datagram Protocol (UDP)** transmissions rather than **Transmission Control Protocol (TDP)** ones. This is an extremely interesting possibility as UDP is much faster than TCP. Swoole also includes a timer implementation accurate to the millisecond, as well as async clients for MySQL, PostgreSQL, Redis, and cURL. The Swoole extension also gives you the ability to set up **inter-process communication (IPC)** using **Golang** style channels. Let's now have a look at installing Swoole.

# Installing the Swoole extension

The Swoole extension can be installed using the same techniques you would deploy to install any PHP extension written in the C language. One approach is to simply use your OS package manager. Examples include `apt` (or its less friendly cousin, `apt-get`) for Debian or Ubuntu Linux, and `yum` or `dnf` for Red Hat, CentOS, or Fedora. When using an OS package manager, the Swoole extension is made available in the form of precompiled binaries.

The recommended approach, however, is to use the `pecl` command. If this command is not available on your installation, the `pecl` command can be installed (logged in as a root user) as follows on an Ubuntu or Debian OS: `apt install php-pear`. For a Red Hat, CentOS, or Fedora installation, the following works: `yum install php-pear`.

When installing the Swoole extension using `pecl`, you can specify a number of options. These options are summarized here: