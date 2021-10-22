2021-02-04 13:22
#tag #bs
# BeautifulSoup
### Поиск по классу CSS
```python
elements = soup.select("p.class.class1")	# выбрать все элементы
element = soup.select_one("p.class.class1 div")  # выбрать ONE элемент
element = soup.select_one("p.class.class1").text # выбрать текст внутри эл-та
element = soup.select_one(".class").find_next_sibling('tag')
# ещес следующий элемент с тегом => tag
text = soup_element.string 					# выберет текст из всего элемента
```
#### [ Фильтры для поиска](https://code.tutsplus.com/ru/tutorials/scraping-webpages-in-python-with-beautiful-soup-search-and-dom-modification--cms-28276)
```python
# получить нужный тэг
for i in all_products_soup:  
	product_link = "https://www.avito.ru/" + i.get("href")  
    product_name = i.get("title")
```
[Документация Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/bs4/doc.ru/bs4ru.html#beautiful-soup "Permalink to this headline")
[Руководство по синтаксическому анализу HTML с помощью BeautifulSoup в Python](https://dev-gang.ru/article/rukovodstvo-po-sintaksiczeskomu-analizu-html-s-pomosczu-beautifulsoup-v-python-kgnmwzixct/)
[BeautifulSoup – парсинг HTML в Python на примерах](https://python-scripts.com/beautifulsoup-html-parsing)

_____________
#### Links
[[Python]] [[Parser]]