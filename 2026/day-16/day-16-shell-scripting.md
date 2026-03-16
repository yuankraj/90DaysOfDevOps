# Day 16 – Shell Scripting Basics

## Objective
Learn the fundamentals of shell scripting including shebang, variables, user input, and conditional statements using Bash.

---

# Task 1 – Your First Script

### Script: hello.sh

```bash
#!/bin/bash

echo "Hello, DevOps!"
```

### Make Script Executable

```bash
chmod +x hello.sh
```

### Run Script

```bash
./hello.sh
```

Example Output

```
Hello, DevOps!
```

### What happens if you remove the shebang?

If the shebang line (`#!/bin/bash`) is removed, the script may still run if executed using `bash hello.sh`.  
However, if you run it directly (`./hello.sh`), the system may not know which interpreter to use.

The shebang tells the system **which shell should interpret the script**.

---

# Task 2 – Variables

### Script: variables.sh

```bash
#!/bin/bash

NAME="Vishal"
ROLE="DevOps Engineer"

echo "Hello, I am $NAME and I am a $ROLE"
```

### Run Script

```bash
chmod +x variables.sh
./variables.sh
```

Example Output

```
Hello, I am Vishal and I am a DevOps Engineer
```

### Single Quotes vs Double Quotes

Example:

```bash
echo '$NAME'
echo "$NAME"
```

Output

```
$NAME
Vishal
```

Explanation:

| Quotes | Behavior |
|------|------|
| Single quotes `' '` | Variables are **not expanded** |
| Double quotes `" "` | Variables **are expanded** |

---

# Task 3 – User Input with read

### Script: greet.sh

```bash
#!/bin/bash

read -p "Enter your name: " NAME
read -p "Enter your favourite tool: " TOOL

echo "Hello $NAME, your favourite tool is $TOOL"
```

### Run Script

```bash
chmod +x greet.sh
./greet.sh
```

Example Output

```
Enter your name: Vishal
Enter your favourite tool: Docker
Hello Vishal, your favourite tool is Docker
```

---

# Task 4 – If-Else Conditions

### Script: check_number.sh

```bash
#!/bin/bash

read -p "Enter a number: " NUM

if [ $NUM -gt 0 ]; then
    echo "The number is positive"
elif [ $NUM -lt 0 ]; then
    echo "The number is negative"
else
    echo "The number is zero"
fi
```

Example Output

```
Enter a number: 5
The number is positive
```

---

### Script: file_check.sh

```bash
#!/bin/bash

read -p "Enter filename: " FILE

if [ -f "$FILE" ]; then
    echo "File exists"
else
    echo "File does not exist"
fi
```

Example Output

```
Enter filename: notes.txt
File exists
```

---

# Task 5 – Combine Everything

### Script: server_check.sh

```bash
#!/bin/bash

SERVICE="nginx"

read -p "Do you want to check the status of $SERVICE? (y/n): " ANSWER

if [ "$ANSWER" = "y" ]; then
    STATUS=$(systemctl is-active $SERVICE)

    if [ "$STATUS" = "active" ]; then
        echo "$SERVICE is running."
    else
        echo "$SERVICE is not running."
    fi
else
    echo "Skipped."
fi
```

### Run Script

```bash
chmod +x server_check.sh
./server_check.sh
```

Example Output

```
Do you want to check the status of nginx? (y/n): y
nginx is running.
```

---

# Scripts Created

```
hello.sh
variables.sh
greet.sh
check_number.sh
file_check.sh
server_check.sh
```

---

# Commands Used

```
chmod
echo
read
if
systemctl
```

---

# What I Learned

1. The **shebang (`#!/bin/bash`) tells Linux which interpreter should run the script**.
2. Variables store values and can be referenced using `$variable_name`.
3. Conditional statements (`if`, `elif`, `else`) allow scripts to make decisions.

---

# Summary

In this exercise, I wrote several Bash scripts to understand scripting fundamentals including variables, user input, and conditional logic. Shell scripting helps automate repetitive tasks and is a core skill for DevOps engineers.
