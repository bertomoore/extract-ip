# extract-ip

Parse a file for IPv4 addresses, then output them to stdout or a file (default option is both).

The inspiration came from copying/pasting the IP address list on OSCP challenges. Instead of copying each IP address one-by-one, it made more sense to copy all of the text (IP descriptions and all) to a raw file and have a script grab them for me.

There are certainly plenty of regex examples on the internet for IP addresses. Unfortunately, most of them capture *anything* within the 0.0.0.0-255.255.255.255 range. That approach might be good enough for most users, but not *this* user, damn it!

## Extracted IP ranges
The default regex captures the following ranges:
- **0.0.0.0/8** (*"This network"*)
- **10.0.0.0/8** (*Private-Use*)
- **127.0.0.0/8** (*Loopback*)
- **169.254.0.0/16** (*Link Local*)
- **172.16.0.0/12** (*Private-Use*)
- **192.168.0.0/16** (*Private-Use*)
- **255.255.255.255** (*Limited Broadcast*)

There is also a regex to capture **all** IPv4 addresses on the [IANA IPv4 Special-Purpose Address Registry](https://www.iana.org/assignments/iana-ipv4-special-registry/iana-ipv4-special-registry.xhtml), including dummy and reserved addresses.

## Requirements
- Python3
- That's it, really

## Usage
```bash
extract-ip raw.txt

# Display help page
extract-ip -h [--help]

# Append IPs to a file of your choice
extract-ip raw.txt -o [--output] ./path/to/file.txt

# Use delimiter of your choice
extract-ip raw.txt -d [--delimiter] ', '

# Do not write to stdout
extract-ip raw.txt -q [--quiet]

# Do not write file
extract-ip raw.txt -n [--no-file]

# Use all ranges in IANA IPv4 Special-Purpose Address Registry
extract-ip raw.txt -i [--iana]
```

## Just here for the regex?
```python
DEFAULT_REGEX = r"(^|(?<=[^0-9]))172\.(1[6-9]|2[0-9]|3[0-1])(\.(0|2[0-4][0-9]|25[0-5]|1?[0-9]?[0-9])){2}|(169\.254|192\.168)(\.(0|2[0-4][0-9]|25[0-5]|1?[0-9]?[0-9])){2}|(0|10|127)(\.(0|2[0-4][0-9]|25[0-5]|1?[0-9]?[0-9])){3}($|(?![0-9]))"

IANA_ADDRESS_REGISTRY_REGEX = r"(^|(?<=[^0-9]))(((172\.(1[6-9]|2[0-9]|3[0-1])|198\.1[8-9]|100\.(6[4-9]|[7-9][0-9]|1[0-1][0-9]|12[0-7]))(\.(0|2[0-4][0-9]|25[0-5]|1?[0-9]?[0-9])){2})|(198\.51\.100|203\.0\.113|192\.(0\.0|0\.2|31\.196|52\.193|88\.99|175\.48))(\.(0|2[0-4][0-9]|25[0-5]|1?[0-9]?[0-9]))|((192\.168|169\.254)(\.(0|2[0-4][0-9]|25[0-5]|1?[0-9]?[0-9])){2})|(192\.(0\.0|0\.2|31\.196|52\.193|88\.99|175\.48))(\.(0|2[0-4][0-9]|25[0-5]|1?[0-9]?[0-9]))|(0|10|127|24[0-9]|25[0-5])(\.(0|2[0-4][0-9]|25[0-5]|1?[0-9]?[0-9])){3})($|(?![0-9]))"
```