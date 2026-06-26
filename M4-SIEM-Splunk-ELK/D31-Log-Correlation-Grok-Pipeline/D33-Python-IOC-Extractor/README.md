# D33 — Python IOC Extractor with Regex

## Objective
Build a Python script to extract Indicators of Compromise (IOCs) from raw text using regex.

## Environment
- Ubuntu WSL2
- Python 3
- `re` module (standard library)

## What was built
`ioc_extractor.py` — extracts the following IOC types from any text input:
- IPv4 addresses
- URLs (http/https)
- Domains
- MD5 hashes
- SHA256 hashes
- CVE IDs

## Bug Fixed
Removed `.exe` false positive from domain extraction. Added `.edu` and `.gov` to valid TLD list.

## Tools Used
- Python 3
- `re` module

## Key Learning
Regex patterns must be specific enough to avoid false positives. SOC analysts use IOC extractors to automate threat intel gathering from logs and reports.
