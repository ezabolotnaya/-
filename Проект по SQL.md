# Описание проекта

Коронавирус застал мир врасплох, изменив привычный порядок вещей. В свободное время жители городов больше не выходят на улицу, не посещают кафе и торговые центры. Зато стало больше времени для книг. Это заметили стартаперы — и бросились создавать приложения для тех, кто любит читать.


**Задача** — проанализировать базу данных.

В ней — информация о книгах, издательствах, авторах, а также пользовательские обзоры книг. Эти данные помогут сформулировать ценностное предложение для нового продукта.

# Опиание данных

**Таблица books**

Содержит данные о книгах:
- *book_id* — идентификатор книги;
- *author_id* — идентификатор автора;
- *title* — название книги;
- *num_pages* — количество страниц;
- *publication_date* — дата публикации книги;
- *publisher_id* — идентификатор издателя.

**Таблица authors**

Содержит данные об авторах:
- *author_id* — идентификатор автора;
- *author* — имя автора.

**Таблица publishers**

Содержит данные об издательствах:

- *publisher_id* — идентификатор издательства;
- *publisher* — название издательства;

**Таблица ratings**

Содержит данные о пользовательских оценках книг:
- *rating_id* — идентификатор оценки;
- *book_id* — идентификатор книги;
- *username* — имя пользователя, оставившего оценку;
- *rating* — оценка книги.

**Таблица reviews**

Содержит данные о пользовательских обзорах:
- *review_id* — идентификатор обзора;
- *book_id* — идентификатор книги;
- *username* — имя автора обзора;
- *text* — текст обзора.

## Выгрузка данных


```python
# импортируем библиотеки
import pandas as pd
from sqlalchemy import text, create_engine
# устанавливаем параметры
db_config = {'user': 'praktikum_student', # имя пользователя
'pwd': 'Sdf4$2;d-d30pp', # пароль
'host': 'rc1b-wcoijxj3yxfsf3fs.mdb.yandexcloud.net',
'port': 6432, # порт подключения
'db': 'data-analyst-final-project-db'} # название базы данных
connection_string = 'postgresql://{user}:{pwd}@{host}:{port}/{db}'.format(**db_config)
# сохраняем коннектор
engine = create_engine(connection_string, connect_args={'sslmode':'require'})
# чтобы выполнить SQL-запрос, используем Pandas
query = '''SELECT * FROM books LIMIT 5'''
con=engine.connect()
pd.io.sql.read_sql(sql=text(query), con = con)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>book_id</th>
      <th>author_id</th>
      <th>title</th>
      <th>num_pages</th>
      <th>publication_date</th>
      <th>publisher_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>546</td>
      <td>'Salem's Lot</td>
      <td>594</td>
      <td>2005-11-01</td>
      <td>93</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>465</td>
      <td>1 000 Places to See Before You Die</td>
      <td>992</td>
      <td>2003-05-22</td>
      <td>336</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>407</td>
      <td>13 Little Blue Envelopes (Little Blue Envelope...</td>
      <td>322</td>
      <td>2010-12-21</td>
      <td>135</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>82</td>
      <td>1491: New Revelations of the Americas Before C...</td>
      <td>541</td>
      <td>2006-10-10</td>
      <td>309</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>125</td>
      <td>1776</td>
      <td>386</td>
      <td>2006-07-04</td>
      <td>268</td>
    </tr>
  </tbody>
</table>
</div>




```python
def table_info(table):
 
    query = '''SELECT * FROM {} LIMIT 5'''.format(table)
    query_1 = '''SELECT COUNT (*) FROM {}'''.format(table)
    data = pd.io.sql.read_sql(query, con = engine)
    data_shape_table = pd.io.sql.read_sql(query_1, con = engine)
    display (data)
    display (data.info())
```


```python
list = ['books','authors', 'ratings', 'reviews', 'publishers']
for i in list:
    print ('\033[1m' + 'Общая информация', i)
    print('\033[0m')
    table_info(i)
```

    [1mОбщая информация books
    [0m



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>book_id</th>
      <th>author_id</th>
      <th>title</th>
      <th>num_pages</th>
      <th>publication_date</th>
      <th>publisher_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>546</td>
      <td>'Salem's Lot</td>
      <td>594</td>
      <td>2005-11-01</td>
      <td>93</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>465</td>
      <td>1 000 Places to See Before You Die</td>
      <td>992</td>
      <td>2003-05-22</td>
      <td>336</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>407</td>
      <td>13 Little Blue Envelopes (Little Blue Envelope...</td>
      <td>322</td>
      <td>2010-12-21</td>
      <td>135</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>82</td>
      <td>1491: New Revelations of the Americas Before C...</td>
      <td>541</td>
      <td>2006-10-10</td>
      <td>309</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>125</td>
      <td>1776</td>
      <td>386</td>
      <td>2006-07-04</td>
      <td>268</td>
    </tr>
  </tbody>
</table>
</div>


    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 5 entries, 0 to 4
    Data columns (total 6 columns):
     #   Column            Non-Null Count  Dtype 
    ---  ------            --------------  ----- 
     0   book_id           5 non-null      int64 
     1   author_id         5 non-null      int64 
     2   title             5 non-null      object
     3   num_pages         5 non-null      int64 
     4   publication_date  5 non-null      object
     5   publisher_id      5 non-null      int64 
    dtypes: int64(4), object(2)
    memory usage: 368.0+ bytes



    None


    [1mОбщая информация authors
    [0m



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>author_id</th>
      <th>author</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>A.S. Byatt</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Aesop/Laura Harris/Laura Gibbs</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Agatha Christie</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Alan Brennert</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Alan Moore/David   Lloyd</td>
    </tr>
  </tbody>
</table>
</div>


    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 5 entries, 0 to 4
    Data columns (total 2 columns):
     #   Column     Non-Null Count  Dtype 
    ---  ------     --------------  ----- 
     0   author_id  5 non-null      int64 
     1   author     5 non-null      object
    dtypes: int64(1), object(1)
    memory usage: 208.0+ bytes



    None


    [1mОбщая информация ratings
    [0m



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>rating_id</th>
      <th>book_id</th>
      <th>username</th>
      <th>rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>1</td>
      <td>ryanfranco</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1</td>
      <td>grantpatricia</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>1</td>
      <td>brandtandrea</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>2</td>
      <td>lorichen</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>2</td>
      <td>mariokeller</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>


    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 5 entries, 0 to 4
    Data columns (total 4 columns):
     #   Column     Non-Null Count  Dtype 
    ---  ------     --------------  ----- 
     0   rating_id  5 non-null      int64 
     1   book_id    5 non-null      int64 
     2   username   5 non-null      object
     3   rating     5 non-null      int64 
    dtypes: int64(3), object(1)
    memory usage: 288.0+ bytes



    None


    [1mОбщая информация reviews
    [0m



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>review_id</th>
      <th>book_id</th>
      <th>username</th>
      <th>text</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>1</td>
      <td>brandtandrea</td>
      <td>Mention society tell send professor analysis. ...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1</td>
      <td>ryanfranco</td>
      <td>Foot glass pretty audience hit themselves. Amo...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>2</td>
      <td>lorichen</td>
      <td>Listen treat keep worry. Miss husband tax but ...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>3</td>
      <td>johnsonamanda</td>
      <td>Finally month interesting blue could nature cu...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>3</td>
      <td>scotttamara</td>
      <td>Nation purpose heavy give wait song will. List...</td>
    </tr>
  </tbody>
</table>
</div>


    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 5 entries, 0 to 4
    Data columns (total 4 columns):
     #   Column     Non-Null Count  Dtype 
    ---  ------     --------------  ----- 
     0   review_id  5 non-null      int64 
     1   book_id    5 non-null      int64 
     2   username   5 non-null      object
     3   text       5 non-null      object
    dtypes: int64(2), object(2)
    memory usage: 288.0+ bytes



    None


    [1mОбщая информация publishers
    [0m



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>publisher_id</th>
      <th>publisher</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Ace</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Ace Book</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Ace Books</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Ace Hardcover</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Addison Wesley Publishing Company</td>
    </tr>
  </tbody>
</table>
</div>


    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 5 entries, 0 to 4
    Data columns (total 2 columns):
     #   Column        Non-Null Count  Dtype 
    ---  ------        --------------  ----- 
     0   publisher_id  5 non-null      int64 
     1   publisher     5 non-null      object
    dtypes: int64(1), object(1)
    memory usage: 208.0+ bytes



    None


Вывод: выгрузили все таблицы и посмотрели общую информацию о них.

## Задание. Сколько книг вышло после 1 января 2000 года?


```python
query = '''


SELECT COUNT(*)
FROM books
WHERE CAST(publication_date AS TIMESTAMP)>=CAST('2000-01-01' AS TIMESTAMP)

'''
data = pd.io.sql.read_sql(query, con = engine)
print('Вывод: после 01 января 2020 года вышло {} книг. '.format(data.loc[0, 'count']) )
```

    Вывод: после 01 января 2020 года вышло 821 книг. 


## Задание. Для каждой книги посчитать количество обзоров и среднюю оценку.


```python
query = '''


SELECT b.title,
       COUNT (DISTINCT re.review_id), ROUND (avg(r.rating), 2) AS R
FROM reviews AS re
RIGHT OUTER JOIN BOOKS AS b ON re.book_id=b.book_id
LEFT OUTER JOIN RATINGS AS r ON b.book_id = r.book_id
GROUP BY b.book_id
ORDER BY R DESC

'''
data = pd.io.sql.read_sql(query, con = engine)
data.columns = ['Название', 'Количество обзоров', 'Средний рейтинг']
data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1000 entries, 0 to 999
    Data columns (total 3 columns):
     #   Column              Non-Null Count  Dtype  
    ---  ------              --------------  -----  
     0   Название            1000 non-null   object 
     1   Количество обзоров  1000 non-null   int64  
     2   Средний рейтинг     1000 non-null   float64
    dtypes: float64(1), int64(1), object(1)
    memory usage: 23.6+ KB



```python
data.head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Название</th>
      <th>Количество обзоров</th>
      <th>Средний рейтинг</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Arrows of the Queen (Heralds of Valdemar  #1)</td>
      <td>2</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>The Walking Dead  Book One (The Walking Dead #...</td>
      <td>2</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Light in August</td>
      <td>2</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Wherever You Go  There You Are: Mindfulness Me...</td>
      <td>2</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Captivating: Unveiling the Mystery of a Woman'...</td>
      <td>2</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Tai-Pan (Asian Saga  #2)</td>
      <td>2</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>The Adventures of Tom Sawyer and Adventures of...</td>
      <td>1</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Hard Times</td>
      <td>2</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>A Fistful of Charms (The Hollows  #4)</td>
      <td>2</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>In the Hand of the Goddess (Song of the Liones...</td>
      <td>2</td>
      <td>5.0</td>
    </tr>
  </tbody>
</table>
</div>



## Задание. Определить издательство, которое выпустило наибольшее число книг толще 50 страниц — так вы исключите из анализа брошюры.


```python
query = '''


SELECT p.publisher,
       count(b.book_id)
FROM publishers AS p
JOIN BOOKS AS b ON p.publisher_id=b.publisher_id
WHERE b.num_pages > 50
GROUP BY p.publisher_id
ORDER BY count(b.book_id) DESC
LIMIT 1

'''
data = pd.io.sql.read_sql(query, con = engine)
data.columns = ['Название издательства', 'Количество книг']
data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1 entries, 0 to 0
    Data columns (total 2 columns):
     #   Column                 Non-Null Count  Dtype 
    ---  ------                 --------------  ----- 
     0   Название издательства  1 non-null      object
     1   Количество книг        1 non-null      int64 
    dtypes: int64(1), object(1)
    memory usage: 144.0+ bytes



```python
data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Название издательства</th>
      <th>Количество книг</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Penguin Books</td>
      <td>42</td>
    </tr>
  </tbody>
</table>
</div>



Издательство "Penguin Books" **выпустило 42 книги** толще 50 страниц.

## Задание. Определить автора с самой высокой средней оценкой книг, учитывая только книги с 50 и более оценками.


```python
query = '''


SELECT a.author,
       AVG(r.rating) avg_rating
FROM authors AS a
JOIN books AS b ON a.author_id=b.author_id
JOIN ratings AS r ON b.book_id=r.book_id
WHERE b.book_id IN
    (SELECT b.book_id
     FROM books AS b
     JOIN ratings AS r ON b.book_id=r.book_id
     GROUP BY b.book_id
     HAVING count(r.rating_id)>=50)
GROUP BY a.author
ORDER BY avg_rating desc

LIMIT 1

'''
data = pd.io.sql.read_sql(query, con = engine)
data.columns = ['Автор', 'Средний рейтинг']
data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1 entries, 0 to 0
    Data columns (total 2 columns):
     #   Column           Non-Null Count  Dtype  
    ---  ------           --------------  -----  
     0   Автор            1 non-null      object 
     1   Средний рейтинг  1 non-null      float64
    dtypes: float64(1), object(1)
    memory usage: 144.0+ bytes



```python
data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Автор</th>
      <th>Средний рейтинг</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>J.K. Rowling/Mary GrandPré</td>
      <td>4.287097</td>
    </tr>
  </tbody>
</table>
</div>



Авторы J.K. Rowling, Mary GrandPré с самой высокой средней оценкой книг, учитывая только книги с 50 и более оценками, средний балл которых **4,3**.

## Посчитайть среднее количество обзоров от пользователей, которые поставили больше 48 оценок.


```python
query = '''

WITH A AS
  (SELECT username,
          count(review_id)
   FROM reviews
   WHERE username IN
       (SELECT username
        FROM ratings
        GROUP BY username
        HAVING count(rating_id)>48)
   GROUP BY username)
SELECT ROUND (SUM (COUNT) / COUNT(COUNT), 2) AS AVG
FROM A

'''
data = pd.io.sql.read_sql(query, con = engine)
print ('Среднее количество обзоров от пользователей, которые поставили больше 48 оценок: ', data.loc[0, 'avg'])
```

    Среднее количество обзоров от пользователей, которые поставили больше 48 оценок:  24.0


**Дополнительное задание**

А) Выведите таблицу, которая будет содержать информацию, сгруппированную по году публикации:

- количество издательств,
- выпущенных книг и
- сколько всего тысяч страниц было в изданных книгах

(отобразить только те года, в которых издано более 30 книг)


```python
query = '''


SELECT EXTRACT (YEAR FROM publication_date) AS publication_year,
       COUNT (DISTINCT publisher_id) AS count_of_publisher,
       COUNT (*) AS count_of_books,
       SUM (num_pages) / 1000 AS sum_of_thousands_pages
FROM books
GROUP BY publication_year
HAVING COUNT(*) > 30
ORDER BY publication_year asc

'''
data = pd.io.sql.read_sql(query, con = engine)
data.columns = ['Год публикации', 'Количество издательств', 'Количество выпущенных книг', 'Сумма страниц в выпущенных книгах (тыс.)']
data
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Год публикации</th>
      <th>Количество издательств</th>
      <th>Количество выпущенных книг</th>
      <th>Сумма страниц в выпущенных книгах (тыс.)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1999.0</td>
      <td>26</td>
      <td>41</td>
      <td>15</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2000.0</td>
      <td>35</td>
      <td>38</td>
      <td>13</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2001.0</td>
      <td>41</td>
      <td>60</td>
      <td>21</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2002.0</td>
      <td>62</td>
      <td>94</td>
      <td>38</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2003.0</td>
      <td>65</td>
      <td>105</td>
      <td>41</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2004.0</td>
      <td>88</td>
      <td>124</td>
      <td>46</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2005.0</td>
      <td>89</td>
      <td>139</td>
      <td>55</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2006.0</td>
      <td>109</td>
      <td>184</td>
      <td>68</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2007.0</td>
      <td>38</td>
      <td>50</td>
      <td>18</td>
    </tr>
  </tbody>
</table>
</div>



- более 30 книг было выпущено с 1999 по 2007 гг.;
- больше всего издательств (109) было в 2006 году;
- более 100 сниг было выпущено в период с 2003 по 2006 гг.;
- соответсвенно в этот пероид количество страниц в выпущенных книгах было больше всего.

В 2007 году количевтсо издательств сократилось более чем в 2 раза, количество выпущенных книг в 3 раза, в которых было суммарно 18 тысяч страниц.

Б) Выведите в одной таблице два числа — среднюю оценку тех книг, на которые написало отзывов более 3 человек и отдельно среднюю оценку остальных книг, сделайте выводы какой рейтинг больше


```python
query = '''


SELECT 
    ROUND (AVG(CASE WHEN count_of_review > 3 THEN avg_rating END),2) AS avg_rating_more_than_3_reviews,
    ROUND (AVG(CASE WHEN count_of_review <= 3 THEN avg_rating END),2) AS avg_rating_less_than_3_reviews
FROM
    (SELECT re.book_id,
            AVG(ra.rating) AS avg_rating,
            COUNT(review_id) AS count_of_review
    FROM reviews AS re
    JOIN books AS b ON re.book_id = b.book_id
    LEFT JOIN ratings AS ra ON b.book_id=ra.book_id
    GROUP BY re.book_id) AS avg_ra;

'''
data = pd.io.sql.read_sql(query, con = engine)
data.columns = ['Более 3 отзывов', 'Менее 3 отзывов']
print ('Средний рейтинг книг по количеству отзывов')
data
```

    Средний рейтинг книг по количеству отзывов





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Более 3 отзывов</th>
      <th>Менее 3 отзывов</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3.9</td>
      <td>3.82</td>
    </tr>
  </tbody>
</table>
</div>



Средний рейтинг книг с менее 3 отзывами - 3,82, а книги с количеством отзывов более 3, с рейтингом 3,9.

Соотвественно, книги с отзывами более 3 имеют средний рейтинг выше, чем остальные, но незначительно.

В) Выведите топ семь пользователей по суммарному показателю написанных ревью и поставленных оценок, указывая суммарное количество страниц в книгах, на которые они отреагировали


```python
query = '''


SELECT ra.username,
       COUNT (re.review_id) AS count_of_reviews,
       SUM (ra.rating) AS sum_of_rating,
       SUM (b.num_pages) AS sum_of_pages
FROM ratings AS ra
JOIN reviews AS re ON ra.username=re.username
JOIN books AS b ON b.book_id=re.book_id
GROUP BY ra.username
ORDER BY (COUNT(re.review_id) + SUM(ra.rating)) DESC

LIMIT 7

'''
data = pd.io.sql.read_sql(query, con = engine)
data.columns = ['Имя пользователя', 'Количество отзывов', 'Сумма оценок', 'Количество страниц в книнах']
data
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Имя пользователя</th>
      <th>Количество отзывов</th>
      <th>Сумма оценок</th>
      <th>Количество страниц в книнах</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>sfitzgerald</td>
      <td>1540</td>
      <td>5908</td>
      <td>533500</td>
    </tr>
    <tr>
      <th>1</th>
      <td>martinadam</td>
      <td>1512</td>
      <td>5724</td>
      <td>607600</td>
    </tr>
    <tr>
      <th>2</th>
      <td>susan85</td>
      <td>1421</td>
      <td>5394</td>
      <td>617645</td>
    </tr>
    <tr>
      <th>3</th>
      <td>richard89</td>
      <td>1430</td>
      <td>5226</td>
      <td>552420</td>
    </tr>
    <tr>
      <th>4</th>
      <td>jennifermiller</td>
      <td>1325</td>
      <td>5075</td>
      <td>479385</td>
    </tr>
    <tr>
      <th>5</th>
      <td>lesliegibbs</td>
      <td>1300</td>
      <td>4836</td>
      <td>492150</td>
    </tr>
    <tr>
      <th>6</th>
      <td>paul88</td>
      <td>1232</td>
      <td>4818</td>
      <td>440216</td>
    </tr>
  </tbody>
</table>
</div>



В таблице выведены ТОП-7 самых активных пользователей.

- больше всего отзывов оставил пользователь sfitzgerald	 - 1540;
- сумма оценок больше у пользователя sfitzgerald - 5908;
- больше всего страниц прочитал и оставил отзывы пользователь susan85 - 617645.

## Общий вывод:

**Задача** — проанализировать базу данных.

В ней — информация о книгах, издательствах, авторах, а также пользовательские обзоры книг. Эти данные помогут сформулировать ценностное предложение для нового продукта.

- После 01 января 2020 г. вышло 821 книг;
- Для каждой книги (1000) было просчитано количество обзоров и средняя оценка;
- Издательство "Penguin Books" выпустило 42 книги толще 50 страниц;
- Авторы J.K. Rowling, Mary GrandPré с самой высокой средней оценкой книг, учитывая только книги с 50 и более оценками, средний балл которых 4,3;
- Среднее количество обзоров от пользователей, которые поставили больше 48 оценок:  24.

**Выводы по дополнительному заданию.**

- более 30 книг было выпущено с 1999 по 2007 гг.;
- больше всего издательств (109) было в 2006 году;
- более 100 сниг было выпущено в период с 2003 по 2006 гг.;
- соответсвенно в этот пероид количество страниц в выпущенных книгах было больше всего.

В 2007 году количевтсо издательств сократилось более чем в 2 раза, количество выпущенных книг в 3 раза, в которых было суммарно 18 тысяч страниц.

Средний рейтинг книг с менее 3 отзывами - 3,82, а книги с количеством отзывов более 3, с рейтингом 3,9. Соотвественно, книги с отзывами более 3 имеют средний рейтинг выше, чем остальные, но незначительно.

Были определены ТОП-7 самых активных пользователей.

- больше всего отзывов оставил пользователь sfitzgerald	 - 1540;
- сумма оценок больше у пользователя sfitzgerald - 5908;
- больше всего страниц прочитал и оставил отзывы пользователь susan85 - 617645.
