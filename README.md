# Auto-sslscan

Includes lazy hacky fix for multiple instances via xargs to work

Auto-sslscan is a python script designed to automate the process of conducting ssl scanning via sslcan (https://github.com/rbsec/sslscan).

The Auto-sslscan script parses an nmap.xml output file, extracts all SSL services and automatically performs an sslscan of them.


## Installation

git clone https://github.com/Moonpenguins/auto-sslscan/

### Prerequisites 

The pre-reqs are:

1. Python (tested on Python 2.7) 
2. You must have sslscan installed and in your path (default in Kali)
3. You need a valid Nmap XML output file (see below)
4. You need python-libnmap (see below)

### Installing python-libnmap

You can install libnmap via pip:
```bash
pip install libnmap
```

or via git:
```bash
$ git clone https://github.com/savon-noir/python-libnmap.git
$ cd python-libnmap
$ python setup.py install
```

### Nmap XML File

The Nmap XML file must have been created with version scanning enabled i.e. via Nmap flags `-sV` or `-A` (see below) 

```bash
nmap -A -p 1-65535 -iL targets.txt -oX nmap-output.xml 
nmap -sS -sV -p 1-65535 -iL targets.txt -oX nmap-output.xml
```

## Usage

```bash
./auto-sslscan.py 
auto-sslscan - v0.1 ( https://github.com/attackdebris/auto-sslscan )

USAGE: auto-sslscan.py [nmap-ouput.xml] [output-file]
```

### Example
```bash
./auto-sslscan.py nmap-output.xml outfile.txt
auto-sslscan - v0.1 ( https://github.com/attackdebris/auto-sslscan )

Performing sslscan of 185.176.90.16:443
Performing sslscan of 199.101.100.186:31337

sslscan results saved to: outfile.txt
SSL services list saved to: ssl-services.txt
```

### Output / Results

The output from the script is a concatenated file (see below)

```bash
cat outfile.txt 

====================================================================
auto-sslscan - v0.1 ( https://github.com/attackdebris/auto-sslscan )
====================================================================

Version: 1.11.10-static
OpenSSL 1.0.2-chacha (1.0.2g-dev)

Testing SSL server 185.176.90.16 on port 443 using SNI name 

  TLS Fallback SCSV:
Server supports TLS Fallback SCSV

  TLS renegotiation:
Secure session renegotiation supported

  TLS Compression:
Compression disabled

  Heartbleed:
TLS 1.2 not vulnerable to heartbleed
TLS 1.1 not vulnerable to heartbleed
TLS 1.0 not vulnerable to heartbleed

  Supported Server Cipher(s):
Preferred TLSv1.2  256 bits  ECDHE-RSA-AES256-GCM-SHA384   Curve P-256 DHE 256
Accepted  TLSv1.2  256 bits  ECDHE-RSA-AES256-SHA384       Curve P-256 DHE 256
Accepted  TLSv1.2  256 bits  ECDHE-RSA-AES256-SHA          Curve P-256 DHE 256
---snip---
```

### Credit

The base code I used to create this: https://github.com/DanMcInerney/nmap-parser/blob/master/nmap-parser.py
