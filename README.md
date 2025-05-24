## 🔍 Katana Hidden Parameter Extractor

This Python script is designed to extract hidden input parameters from HTML responses found in `katana.jsonl` output produced by [projectdiscovery/katana](https://github.com/projectdiscovery/katana).

It performs the following:

- Parses all unique `endpoint` URLs from the Katana JSONL file.
- Sends concurrent HTTP GET requests to each URL.
- Identifies hidden form fields (`<input type="hidden" name="...">`) in the page.
- Generates GET URLs with those hidden parameters (e.g., `example.com?token=enumrust&session=enumrust`).
- Saves the results to `katana_hidden_params_fuzzing.txt` for further fuzzing or testing.
- Useful for bug bounty hunters and web security testers who want to fuzz endpoints with auto-discovered hidden parameters.
```bash
$ katana -u http://testphp.vulnweb.com -aff -j -o katana.jsonl
$ python3 hidden-param-extractor.py
```
Using a URL list for security testing can be painful as there are a lot of URLs that have uninteresting/duplicate content; [uro](https://github.com/s0md3v/uro) aims to solve that.
```bash
uro -i katana_hidden_params_fuzzing.txt -o katana_urls_with_hidden_params.txt
```
---
#### 🚀 Nuclei DAST Scan Setup

##### 1. Clone Fast Templates Repository:
```bash
git clone https://github.com/reewardius/nuclei-fast-templates
```
##### 2. Run the DAST Scan using Nuclei:
Make sure you have `nuclei` installed. Then run the following command to perform a scan using the DAST templates:
```bash
nuclei -l katana_urls_with_hidden_params.txt -tags fuzzing-req -dast -t nuclei-fast-templates/ -o hidden_params_dast_results.txt
```

#### ✨ Arjun

This phase involves discovering hidden GET parameters using Arjun.

##### 3. Extract URLs with backend extensions using Katana

First, use Katana to crawl URLs and filter those with common server-side script extensions:
```bash
katana -u http://testphp.vulnweb.com -o - | awk -F'\\?' '{print $1}' | sort -u | grep -Ei '\.(php|asp|aspx|ashx|jsp|jspx|cgi|pl|py|rb|cfm)$' > katana_links_with_extensions.txt
```
##### 4. Discover hidden parameters with Arjun

Next, run Arjun on the filtered URLs to identify hidden GET parameters:
```bash
arjun -i katana_links_with_extensions.txt -t 15 -oT arjun_results.txt
```
##### 3. Run the DAST Scan using Nuclei:

Use the discovered parameterized URLs to perform a dynamic scan with Nuclei:
```bash
nuclei -l arjun_results.txt -tags fuzzing-req -dast -t nuclei-fast-templates/ -o arjun_params_dast_results.txt
```
