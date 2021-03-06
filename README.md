# Backoffice Demo

This repository shows how to implement Formal SqlCommenter SDK in your back office application.

## How ?

You can integrate Formal in your back office application in a couple of lines of code by using our SQLCommenter library. The SQLCommenter library will augment your SQL queries with a comment containing an end_user_id, allowing the Formal Sidecar to apply policies to your users.

## Example

If we query the api with the `endUserID=5`, then we will be able to retrieve this log on Formal console. 
```
GET endpoint/?endUserID=5
```
![image](https://user-images.githubusercontent.com/43049559/143810587-f26274d9-a1dd-4477-a44a-581dee595021.png)

## Implementation 

### Python

We have built a [wrapper (formal sql-commenter library)](https://github.com/formalco/sqlcommenter) around `psycopg2` so that you can add easily integrate Formal to your python apis.

```python
import psycopg2
from formal.sqlcommenter.psycopg2.extension import CommenterCursorFactory
import os

REACT_APP_COMPANY_NAME
host = os.getenv('DATABASE_URL')
dbName = os.getenv('DATABASE_NAME')
user = os.getenv('DATABASE_USER')
password = os.getenv('DATABASE_PASSWORD')

cursor_factory = CommenterCursorFactory()
conn = psycopg2.connect(
    host=host,
    database=dbName,
    user=user,
    password=password,
    cursor_factory=cursor_factory)
cursor = conn.cursor()


@app.route('/api/v1/fetch-all', methods=["GET"])
def fetch():
    if 'endUserID' in request.args:
        endUserID = int(request.args['endUserID'])
    else:
        return "Error: No end user id field provided. Please specify an endUserID."
    try:
        cursor.execute("select * from pii;", endUserID)
        return jsonify(cursor.fetchall())
    except:
        return "Error: an error occured. Please try again."

```

## Build
Building the project is as simple as building a docker container :
```
docker build --build-arg company_name=[your_company] --build-arg api_uri=http://127.0.0.1:4000/api/v1 --build-arg logo=[your_logo_url] --build-arg primary_color=[your_company_1st_color] --build-arg secondary_color=[your_company_2nd_color] --build-arg third_color=[your_company_3rd_color]  -t formal-demo-backend .
```

## Run
Running the project can be done after building the docker container AND deploying a Formal proxy on (our console)[https://joinformal.com]:
```
docker run -e  DATABASE_URL=[formal_proxy_url] -e DATABASE_NAME=main -e  DATABASE_PASSWORD=[db_pwd] -e DATABASE_USER=[db_user] -p 8080:8080 -p 4000:4000 formal-demo-backend
```

## Support

Here are the backend languages currently supported :

- [X] python
- [ ] nodeJS
- [ ] ruby
- [ ] java
- [ ] golang
