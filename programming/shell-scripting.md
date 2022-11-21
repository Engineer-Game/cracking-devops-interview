# Shell Scripting

## Understanding I/O

The numbers are file descriptors and only the first three (starting with zero) have a standardized meaning:

```
0
-
 stdin

1
-
 stdout

2
-
 stderr
```

So each of these numbers in your command refer to a file descriptor. You can either redirect a file descriptor to a file with`>`or redirect it to another file descriptor with`>&`

The`3>&1`in your command line will create a new file descriptor and redirect it to`1`which is`STDOUT`. Now`1>&2`will redirect the file descriptor 1 to`STDERR`and`2>&3`will redirect file descriptor 2 to 3 which is`STDOUT`.

So basically you switched`STDOUT`and`STDERR`, these are the steps:

1. Create a new fd 3 and point it to the fd 1
2. Redirect file descriptor 1 to file descriptor 2. If we wouldn't have saved the file descriptor in 3 we would lose the target.
3. Redirect file descriptor 2 to file descriptor 3. Now file descriptors one and two are switched.

Now if the program prints something to the file descriptor 1, it will be printed to the file descriptor 2 and vice versa.

## Extracting relevant data from logs

Let's imagine our web server is receiving a large amount of queries and this is affecting teh performance.

We can display the ip which are performing more queries with this command:

cat access.log | awk '{print $1}' | sort -n | uniq -c | sort -nr | head -20

Find and rename folders/files given a regex

Log parsing in a folder with logs 2015/20/10/10/300

what is command1 && command2

what is command1 || command2

\$$ - Pid Process

```
bash4$ echo $$
11015
bash4$ echo $BASHPID
11015
```

```
$! is PID of last job running in background
```

$\* expands to a single argument with all the elements delimited by spaces (actually the first character of $IFS).

$@ expands to multiple arguments.

```
#!/bin/bash
echo "With *:"
for arg in "$*"; do echo "<$arg>"; done
echo
echo "With @:"
for arg in "$@"; do echo "<$arg>"; done
```

### Operator Precedence

### [http://www.tldp.org/LDP/abs/html/opprecedence.html](http://www.tldp.org/LDP/abs/html/opprecedence.html)

```
if [ -n "${10}" ]  # Parameters > $9 must be enclosed in {brackets}.
then
 echo "Parameter #10 is ${10}"
fi
```

```
Variable    Use
$#    Stores the number of command-line arguments that were passed to the shell program.
$?    Stores the exit value of the last command that was executed.
$0    Stores the first word of the entered command (the name of the shell program).
$*    Stores all the arguments that were entered on the command line ($1 $2 ...).
"$@"    Stores all the arguments that were entered on the command line, individually quoted ("$1" "$2" ...).
```

### Arrays

[http://tldp.org/LDP/abs/html/arrays.html](http://tldp.org/LDP/abs/html/arrays.html)

### Shift

```
echo "$@"    # 1 2 3 4 5
shift
```

Reverse a String arguments (ex: Hello world prints world hello - take in consideration spaces

[https://www.digitalocean.com/community/tutorials/how-to-use-bash-history-commands-and-expansions-on-a-linux-vps](https://www.digitalocean.com/community/tutorials/how-to-use-bash-history-commands-and-expansions-on-a-linux-vps)

### Command to create an empty file

touch
