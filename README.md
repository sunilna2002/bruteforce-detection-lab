# SSH Log Analysis Lab – Simulated Brute Force Detection

## Project Overview

This project demonstrates how to simulate and analyze SSH authentication logs using Linux command-line tools. The goal was to generate realistic failed and successful SSH login attempts, then investigate the logs using commands such as `grep`, `awk`, `sort`, `uniq`, and `head`.

The project was completed as part of hands-on cybersecurity practice after completing the Google Cybersecurity Certificate.

---

## Skills Demonstrated

* Linux command-line usage
* Log analysis
* SSH authentication monitoring
* Pattern matching with `grep`
* Field extraction using `awk`
* Frequency analysis using `sort` and `uniq`
* Basic threat hunting concepts
* Simulating brute-force login attempts

---

## Project Objectives

* Generate simulated SSH failed login attempts
* Generate simulated successful login events
* Store events inside an `auth.log` file
* Analyze suspicious IP addresses
* Identify high-frequency login attempts
* Practice real-world SOC analyst style investigations

---

## Tools & Environment

| Tool                  | Purpose                     |
| --------------------- | --------------------------- |
| Ubuntu/Linux Terminal | Log generation and analysis |
| Bash Shell            | Automation                  |
| grep                  | Pattern searching           |
| awk                   | Field extraction            |
| sort                  | Sorting data                |
| uniq                  | Counting duplicate entries  |
| head                  | Displaying top results      |

---

# Step 1 – Simulating Failed SSH Login Attempts

The following Bash loop generated 5000 failed SSH login entries:

```bash
for i in {1..5000}; do
  echo "$(date) Failed Password for root from 185.220.226.$((RANDOM%255)) port $((RANDOM%65535)) SSH2" >> auth.log
 done
```

### Explanation

* `for i in {1..5000}` → Runs the loop 5000 times
* `$(date)` → Inserts the current timestamp
* `RANDOM%255` → Generates random IP octets
* `RANDOM%65535` → Generates random port numbers
* `>> auth.log` → Appends logs into the file

This simulates a brute-force SSH attack targeting the `root` account.

---

# Step 2 – Simulating Successful SSH Logins

The following loop generated successful SSH login events:

```bash
for i in {1..5000}; do
  echo "$(date) Login successful for root from 185.220.129.$((RANDOM%255)) Port $((RANDOM%65535)) SSH2" >> auth.log
 done
```

This creates realistic successful login records for analysis.

---

# Step 3 – Viewing the Logs

```bash
cat auth.log
```

This command displays all generated authentication logs.

---

# Step 4 – Finding Successful Logins

```bash
grep -i "successful" auth.log
```

### Explanation

* `grep` searches for matching text
* `-i` makes the search case-insensitive

---

# Step 5 – Extracting Source IP Addresses

```bash
grep -i "successful" auth.log | awk '{print $(NF-3)}'
```

### Explanation

* `NF` means “Number of Fields”
* `$(NF-3)` extracts the IP address field from the log

---

# Step 6 – Counting Login Frequencies

```bash
grep -i "successful" auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr
```

### What This Does

1. Finds successful login entries
2. Extracts IP addresses
3. Sorts them alphabetically
4. Counts occurrences using `uniq -c`
5. Sorts numerically in descending order

This helps identify the most active IP addresses.

---

# Step 7 – Displaying Top Results

```bash
grep -i "successful" auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr | head -10
```

This displays the top 10 most frequent successful login IPs.

---

## Sample Findings

| Observation                | Description                             |
| -------------------------- | --------------------------------------- |
| Multiple failed logins     | Indicates possible brute-force behavior |
| Randomized IP addresses    | Simulates distributed attacks           |
| Frequency analysis         | Helps identify suspicious hosts         |
| Command-line investigation | Demonstrates SOC analyst workflows      |

---

## Screenshots



* `screenshots/failed-logins.png`
* `screenshots/successful-logins.png`

---

## Folder Structure

```text
bruteforce-detection-Lab/
│
├── README.md
├── auth.log
├── screenshots/
│   ├── failed-logins.png
│   └── successful-logins.png
└── commands.txt
```

---

## commands.txt Example

```text
# Generate failed logins
for i in {1..5000}; do
  echo "$(date) Failed Password for root from 185.220.226.$((RANDOM%255)) port $((RANDOM%65535)) SSH2" >> auth.log
 done

# Generate successful logins
for i in {1..5000}; do
  echo "$(date) Login successful for root from 185.220.129.$((RANDOM%255)) Port $((RANDOM%65535)) SSH2" >> auth.log
 done

# Find successful logins
grep -i "successful" auth.log

# Extract IPs
grep -i "successful" auth.log | awk '{print $(NF-3)}'

# Count frequency
grep -i "successful" auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr
```

---

## Key Learning Outcomes

* Learned how authentication logs are structured
* Practiced detecting brute-force style behavior
* Improved Linux command-line proficiency
* Understood how SOC analysts investigate login activity
* Built hands-on cybersecurity portfolio experience

---

## Future Improvements

* Automate log analysis using Bash scripts
* Visualize attack trends using Python
* Parse real `/var/log/auth.log` files
* Create alerts for suspicious login thresholds
* Export findings into CSV reports

---


---

## Author

Sunil Rajpal

GitHub Portfolio:

Sunil Rajpal Cyber Portfolio[https://github.com/sunilna2002/Sunil-Rajpal-Cyber-Portfolio](https://github.com/sunilna2002/Sunil-Rajpal-Cyber-Portfolio)

---
