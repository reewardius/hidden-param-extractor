## 🔍 Katana Hidden Parameter Extractor

This Python script is designed to extract hidden input parameters from HTML responses found in `katana.jsonl` output produced by [projectdiscovery/katana](https://github.com/projectdiscovery/katana).

It performs the following:

- Parses all unique `endpoint` URLs from the Katana JSONL file.
- Sends concurrent HTTP GET requests to each URL.
- Identifies hidden form fields (`<input type="hidden" name="...">`) in the page.
- Generates GET URLs with those hidden parameters (e.g., `example.com?token=enumrust&session=enumrust`).
- Saves the results to `katana_hidden_params_fuzzing.txt` for further fuzzing or testing.
- Useful for bug bounty hunters and web security testers who want to fuzz endpoints with auto-discovered hidden parameters.
```
$ katana -u http://testphp.vulnweb.com -aff -j -o katana.jsonl
$ python3 hidden-param-extractor.py
```
Using a URL list for security testing can be painful as there are a lot of URLs that have uninteresting/duplicate content; [uro](https://github.com/s0md3v/uro) aims to solve that.
```
uro -i katana_hidden_params_fuzzing.txt -o katana_urls_with_hidden_params.txt
```
---
#### 🚀 Nuclei DAST Scan Setup

##### 1. Clone Fast Templates Repository:
```
git clone https://github.com/reewardius/nuclei-fast-templates
```
##### 2. Run the DAST Scan:
Make sure you have `nuclei` installed. Then run the following command to perform a scan using the DAST templates:
```
nuclei -l katana_urls_with_hidden_params.txt -t nuclei-fast-templates/ -dast -etags fuzzing-req-header,fuzzing-req-cookie,fuzzing-req-path
```
