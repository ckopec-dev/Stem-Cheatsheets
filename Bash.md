
# Bash Cheat Sheet

## Control statements

### Basic IF statement syntax

~~~

if [ CONDITION ]; then
      # CODE TO EXECUTE IF TRUE
else 
      # CODE TO EXECUTE IF FALSE
fi

# For numerical comparisons, there are two options for declaring the condition:
if ((CONDITION)); then # This option uses standard numerical comparison operators, e.g. '>'
if [ CONDITION ]; then # This option requires comparison operator flags, e.g. '-gt'

# Commands can be executed in the condition by excluding the brackets, e.g.:
if grep -q SomeText MyFile.txt; then

# Execute via shell-within-a-shell technique. Produces same result as above:
if $(grep -q SomeText MyFile.txt); then

~~~

### Multiple condition examples

~~~

if [ CONDITION A ] && [ CONDITION B ]; then # A AND B
if [ CONDITION A && CONDITION B ]; then # A AND B (same as above)
if [ CONDITION A ] || [ CONDITION B ]; then # A OR B
if [ CONDITION A || CONDITION B ]; then # A OR B (same as above)

~~~

### Special conditional flags

| Flag | Description |
| ---- | ----------- |
| -e | if the file exists |
| -s | if the file exists and the the size is greater than 0 |
| -r | if the file exists and is readable |
| -w | if the file exists and is writable |

### Arithmetic conditional flags
| Flag | Description |
| ---- | ----------- |
| -eq | equal to |
| -ne | not equal to |
| -lt | less than |
| -le | less than or equal to |
| -gt | greater than |
| -ge | greater than or equal to |

### Basic FOR statement syntax

~~~

for ITEM in ITEMS
do
      # Do something
done

# Numeric iteration
for i in {START..STOP..INCREMENT} # Start defaults to 1 if not specified.

# Three expression syntax example (very similar to c syntax):
for i in ((i=2;i<=4;i++))

# File iteration example:
for logfile in logfiles/*

# Shell-in-a-shell example:
for loggile in $(ls logfiles/ | grep -i 'ftp')

~~~

### Basic WHILE statement syntax

~~~

while [ CONDITION ]; # Syntax is similar to IF condtition syntax.
do
      # Do something
done

~~~

### Basic CASE statement syntax

~~~

case STRING OR VARIABLE in
      PATTERN1)
      COMMAND1;;
      PATTERN2)
      COMMAND2;;
      *)
      DEFAULT COMMAND;;
esac

~~~
