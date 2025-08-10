
reference: https://dev.mysql.com/doc/refman/8.4/en/

```
Type '\help' or '\?' for help; '\quit' to exit.
SHELL COMMANDS

The shell commands allow executing specific operations including updating the
shell configuration.

The following shell commands are available:

 - \                   Start multi-line input when in SQL mode.
 - \connect    (\c)    Connects the shell to a MySQL server and assigns the
                       global session.
 - \disconnect         Disconnects the global session.
 - \edit       (\e)    Launch a system editor to edit a command to be executed.
 - \exit               Exits the MySQL Shell, same as \quit.
 - \help       (\?,\h) Prints help information about a specific topic.
 - \history            View and edit command line history.
 - \js                 Switches to JavaScript processing mode.
 - \nopager            Disables the current pager.
 - \nowarnings (\w)    Don't show warnings after every statement.
 - \option             Allows working with the available shell options.
 - \pager      (\P)    Sets the current pager.
 - \py                 Switches to Python processing mode.
 - \quit       (\q)    Exits the MySQL Shell.
 - \reconnect          Reconnects the global session.
 - \rehash             Refresh the autocompletion cache.
 - \show               Executes the given report with provided options and
                       arguments.
 - \source     (\.)    Loads and executes a script from a file.
 - \sql                Executes SQL statement or switches to SQL processing
                       mode when no statement is given.
 - \status     (\s)    Print information about the current global session.
 - \system     (\!)    Execute a system shell command.
 - \use        (\u)    Sets the active schema.
 - \warnings   (\W)    Show warnings after every statement.
 - \watch              Executes the given report with provided options and
                       arguments in a loop.

GLOBAL OBJECTS

The following modules and objects are ready for use when the shell starts:

 - dba     Used for InnoDB Cluster, ReplicaSet, and ClusterSet administration.
 - mysql   Support for connecting to MySQL servers using the classic MySQL
           protocol.
 - mysqlx  Used to work with X Protocol sessions using the MySQL X DevAPI.
 - os      Gives access to functions which allow to interact with the operating
           system.
 - plugins Plugin to manage MySQL Shell plugins
 - shell   Gives access to general purpose functions and properties.
 - sys     Gives access to system specific parameters.
 - util    Global object that groups miscellaneous tools like upgrade checker
           and JSON import.

For additional information on these global objects use: <object>.help()

EXAMPLES
\? AdminAPI
      Displays information about the AdminAPI.

\? \connect
      Displays usage details for the \connect command.

\? checkInstanceConfiguration
      Displays usage details for the dba.checkInstanceConfiguration function.

\? sql syntax
      Displays the main SQL help categories.

 ```