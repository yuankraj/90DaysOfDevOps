# Day 06 – Linux Fundamentals: Read and Write Text Files

# Objective

Today I practiced basic Linux file handling commands:
- Creating files
- Writing text into files
- Appending new lines
- Reading complete and partial file content

---

# Step 1 – Create a File

## Input
```bash
touch notes.txt
```

## Output
```bash
File created successfully
```

## Observation
- `touch` creates an empty file quickly.
- Useful for creating config or log files.

---

# Step 2 – Write First Line into File

## Input
```bash
echo "Linux is powerful." > notes.txt
```

## Output
```bash
(No output)
```

## Observation
- `>` overwrites file content.
- First line written successfully.

---

# Step 3 – Append Second Line

## Input
```bash
echo "DevOps uses Linux daily." >> notes.txt
```

## Output
```bash
(No output)
```

## Observation
- `>>` appends new content without deleting old data.

---

# Step 4 – Append Third Line using tee

## Input
```bash
echo "Practice builds confidence." | tee -a notes.txt
```

## Output
```bash
Practice builds confidence.
```

## Observation
- `tee -a` displays and appends text simultaneously.
- Useful during logging and scripting.

---

# Step 5 – Add More Lines

## Input
```bash
echo "Logs are text files." >> notes.txt
echo "Automation starts with basics." >> notes.txt
echo "Linux skills improve troubleshooting." >> notes.txt
echo "Reading files is important." >> notes.txt
echo "Writing files is a daily task." >> notes.txt
```

## Output
```bash
(No output)
```

---

# Step 6 – Read Full File

## Input
```bash
cat notes.txt
```

## Output
```bash
Linux is powerful.
DevOps uses Linux daily.
Practice builds confidence.
Logs are text files.
Automation starts with basics.
Linux skills improve troubleshooting.
Reading files is important.
Writing files is a daily task.
```

## Observation
- `cat` displays the entire file content.
- Helpful for quick file inspection.

---

# Step 7 – Read First 2 Lines

## Input
```bash
head -n 2 notes.txt
```

## Output
```bash
Linux is powerful.
DevOps uses Linux daily.
```

## Observation
- `head` reads the beginning of a file.
- Useful for checking headers or recent edits.

---

# Step 8 – Read Last 2 Lines

## Input
```bash
tail -n 2 notes.txt
```

## Output
```bash
Reading files is important.
Writing files is a daily task.
```

## Observation
- `tail` displays the last lines of a file.
- Commonly used for log monitoring.

---

# Quick Learning

Commands Practiced:
- touch
- echo
- >
- >>
- tee
- cat
- head
- tail

Key Learning:
- Linux handles most configurations and logs as text files.
- Basic file operations are essential for DevOps and troubleshooting.

---

# Summary

Today I learned how to:
- Create files
- Write and append data
- Read full and partial file content
- Use tee for writing and displaying simultaneously

This practice improved my confidence with Linux file handling fundamentals.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
