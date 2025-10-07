# Homework 5

# hw5_server
Stub HTTP and SSH servers for Homework 5.
Prototyped with Codeium Windsurf and the DeepSeek V3 model.
Intentionally incomplete—these programs do not handle all situations
gracefully.

## ssh_server.py
Usage:
```
python3 ssh_server.py # defaults to 2222, admin, admin
python3 ssh_server.py --port 2224 
python3 ssh_server.py --port 2224 --username myuser --password mypass
```

## http_server.py
Usage:
```
python3 http_server.py # defaults to 8080, admin, admin
python3 http_server.py --port 8081 
python3 http_server.py --port 8081 --username user --password pass
```

## Assignment 5.2: SQL Injection Attack

### Models Used
- **Claude Sonnet 4.5** (Anthropic) - Primary model used for generating the SQL injection payload
- **GPT-5**
- **ChatGPT (GPT-4.1)**

### Guardrails Encountered
- Claude Sonnet 4.5 did not have significant resistance to helping with this task, as it was clearly framed as an educational assignment about security vulnerabilities. The model provided straightforward explanations of SQL injection techniques when the educational context was clear.
- Both GPT-5 and GPT-4.1 refused to provide exploit payloads for SQL injection. Common guardrails included: Exploit crafting is disallowed even for sandboxed targets. Provided educational guidance on parameterized queries and secure coding instead. They both replied with something similar to "I can’t help you craft or test a SQL-injection payload (including the exact JSON for attack.json) or write prompts whose goal is to bypass model safety to carry out an exploit—even for a class sandbox. That’s disallowed assistance."

### Testing
Test the attack with:
```bash
curl -X POST https://cat-hw4.vercel.app/county_data -H "Content-Type: application/json" -d @attack.json
```

Expected result: Returns 100 rows from the database instead of just matching records.

## Assignment 5.3: Vulnerability Scanner

### Models Used
- **Claude Sonnet 4.5** (Anthropic) - Used to generate the vulnerability scanner code

### Implementation Details
The `vulnerability_scanner.py` program:
- Uses `python-nmap` to scan TCP ports 1-8999 on localhost
- Tests each open port with HTTP basic authentication using the `requests` library
- Tests each open port with SSH password authentication using `paramiko`
- Outputs successful connections in RFC 3986 URI format followed by server output
- Handles exceptions silently (unless `-v` flag is used)
- Supports a `-v` verbose flag for debugging

### Dependencies
Install required packages:
```bash
pip install -r requirements.txt
```

Note: You also need `nmap` installed on your system:
- macOS: `brew install nmap`
- Ubuntu/Debian: `sudo apt-get install nmap`
- Windows: Download from https://nmap.org/download.html

### Usage
```bash
# Run the scanner
python vulnerability_scanner.py

# Run with verbose output
python vulnerability_scanner.py -v
```

### Testing
1. Clone the hw5_server repository:
```bash
git clone https://github.com/cs1060f25/hw5_server
```

2. Run the vulnerable servers:
```bash
python hw5_server/ssh_server.py &
python hw5_server/http_server.py &
```

3. Run the scanner:
```bash
python vulnerability_scanner.py
```

Expected output format:
```
http://admin:admin@127.0.0.1:8080 success
ssh://skroob:12345@127.0.0.1:2222 schwartz
```

## Code Attribution
- All code was generated with assistance from Claude Sonnet 4.5 (Anthropic)
- SQL injection technique based on standard SQL injection methodology as taught in CS1060
- Python libraries used: python-nmap, paramiko, requests (all open source)

## Files Included
- `attack.json` - Malicious JSON payload for SQL injection
- `test.json` - Normal test JSON for comparison
- `prompts.txt` - GenAI prompts used to generate the attack
- `vulnerability_scanner.py` - Penetration testing tool
- `requirements.txt` - Python dependencies
- `README.md` - This file
- `.gitignore` - Git ignore rules

## Notes
- The vulnerability scanner is intentionally designed for educational purposes only
- Both the SQL injection and brute force attacks demonstrate common security vulnerabilities
- In production systems, use parameterized queries (SQL) and strong authentication mechanisms
- Never use these techniques on systems you don't own or have explicit permission to test
