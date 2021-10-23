2021-10-12 13:08
#tag
# psycopg2
`pip install psycopg2`
```py
import psycopg2

conn = psycopg2.connect(dbname='my_database', user='username')
cursor = conn.cursor()

# Выполняем запрос.
cursor.execute("SELECT * FROM table_name")
row = cursor.fetchone()

# Закрываем подключение.
cursor.close()
conn.close()
```
_____________
#### Links
