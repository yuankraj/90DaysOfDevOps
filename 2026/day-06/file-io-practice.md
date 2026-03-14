# Day 06 – Linux Fundamentals: Read and Write Text Files

## Goal
Practice basic file input/output operations in Linux using simple commands.

---

## 1. Create a File

### Command
```bash
touch notes.txt
```

Observation  
Creates an empty file named **notes.txt** in the current directory.

---

## 2. Write Text to File

### Command
```bash
echo "Learning Linux file operations" > notes.txt
```

Observation  
Writes the first line into the file.  
The **> operator overwrites** the file content.

---

## 3. Append New Lines

### Command
```bash
echo "Practicing redirection operators" >> notes.txt
```

Observation  
Adds a second line to the file.  
The **>> operator appends text** without deleting existing content.

---

### Command
```bash
echo "Using tee command to write and display" | tee -a notes.txt
```

Example Output
```
Using tee command to write and display
```

Observation  
The **tee command writes to the file and prints the text on the terminal at the same time**.

---

## 4. View Full File Content

### Command
```bash
cat notes.txt
```

Example Output
```
Learning Linux file operations
Practicing redirection operators
Using tee command to write and display
```

Observation  
Displays the **entire file content**.

---

## 5. Read First Few Lines

### Command
```bash
head -n 2 notes.txt
```

Example Output
```
Learning Linux file operations
Practicing redirection operators
```

Observation  
Shows the **first 2 lines** of the file.

---

## 6. Read Last Few Lines

### Command
```bash
tail -n 2 notes.txt
```

Example Output
```
Practicing redirection operators
Using tee command to write and display
```

Observation  
Shows the **last 2 lines** of the file.

---

## Final Content of notes.txt

```
Learning Linux file operations
Practicing redirection operators
Using tee command to write and display
```

---

## Commands Used

| Command | Purpose |
|------|------|
| touch | Create a new file |
| echo > | Write content to file |
| echo >> | Append content |
| tee -a | Write and display simultaneously |
| cat | Display entire file |
| head | Display first lines |
| tail | Display last lines |

---

## Summary

In this exercise, I practiced basic Linux file input/output operations.  
I created a file, wrote text using redirection, appended lines, and used commands like **cat, head, tail, and tee** to read file contents efficiently.
