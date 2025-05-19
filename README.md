## 🔍 Katana Hidden Parameter Extractor

This Python script is designed to extract hidden input parameters from HTML responses found in `katana.jsonl` output (produced by [projectdiscovery/katana]([url](https://github.com/projectdiscovery/katana))).

It performs the following:

- Parses all unique `endpoint` URLs from the Katana JSONL file.
- Sends concurrent HTTP GET requests to each URL.
- Identifies hidden form fields (`<input type="hidden" name="...">`) in the page.
- Generates GET URLs with those hidden parameters (e.g., `example.com?token=enumrust&session=enumrust`).
- Saves the results to `katana_hidden_params_fuzzing.txt` for further fuzzing or testing.
- Useful for bug bounty hunters and web security testers who want to fuzz endpoints with auto-discovered hidden parameters.
```
$ katana -u http://testphp.vulnweb.com -aff -j katana.jsonl
$ python3 hidden-param-extractor.py
```
