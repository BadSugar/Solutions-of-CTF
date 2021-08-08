# Traffic Analysis - 3 (100 points)

Challenge description:
> We believe the attacker was able to make a change to our webpage to allow them to gain access into our internal network.
>
> Our Malcolm instance is set to extract files it sees over the network, including HTML files. Extracted files from Malcolm can be found at https://malcolm.cisaicsctf.com/extracted-files/
>
> Can you determine the name of the div within the HTML file that allowed the attacker to gain access?



Opening the link(https://malcolm.cisaicsctf.com/extracted-files/) from the description:

![image-20210808023906881](img/directory listing.png)





Let's open them and see what's inside, shall we?



### preserved:

![image-20210808024124846](img/image-20210808024124846.png)

There's more.



### Quarantine:

![image-20210808024056767](img/quarantine.png)





The HTML files are in the preserved directory, so we will have to check each one and check which one **allowed the attacker to gain access**.



But are we going to open each HTML? No! We can write a program in python that will print the content of each HTML.



```python
# filename: script.py
# written by BadSugar

import requests
from bs4 import BeautifulSoup

MAIN_URL = 'https://malcolm.cisaicsctf.com/extracted-files/preserved/'

headers = {
    'Connection': 'keep-alive',
    'Cache-Control': 'max-age=0',
    'Authorization': 'Basic aWNzandnY3RmOklDU01AbGMwbG0='
}

response = requests.get(MAIN_URL, headers=headers)
soup = BeautifulSoup(response.text)

htmls = []
for elm in [elm.a for elm in soup.findAll('li')]:
    if 'html' in elm['href']:
        htmls.append(MAIN_URL + elm['href'])

for idx, html in enumerate(htmls):
    print("----\n\n")
    print(f"{idx}) - {html}")
    response = requests.get(html, headers=headers).text
    print(response)
    print("\n\n----\n")
```



Let's run it

```
$ python3 script.py
--snip--
</body>
<div name="RadioTower" style="display: none">
#<?php
/*******************************************************************************
 * Copyright 2017 WhiteWinterWolf
--snip--
```



It's look like we found the name of the div which is suspected => **RadioTower**.


~BadSugar
