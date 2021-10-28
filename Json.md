2021-02-26 13:53
#json
# Json 
[Online Viewer](https://jsonformatter.org/json-viewer) 

```python
"Запись в фаил"
with open("dict_name.json", "w") as file:					# имя словаря.json
    json.dump(dict_name, file, indent=4, ensure_ascii=False) 
	# ensure_ascii - false кодирует кирилицу
	# indent 	   - отступ(без него вся запись в одну строку)
```
_____________
#### Links
