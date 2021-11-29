# Backoffice Demo

This repository shows how to implement Formal in your backoffice applications.

## How ?

You can integrate Formal in your backoffice in a couple of lines of code by using our libraries. All the queries made from your backoffice will then be added to Formal logs allowing you to have a better understanding of who uses what on your internal applications.

### Python

We have built a [wrapper (formal sql-commenter library)](https://github.com/formalco/sqlcommenter) around `psycopg2` so that you can add easily integrate Formal to your python apis.

```python
import psycopg2
from formal.sqlcommenter.psycopg2.extension import CommenterCursorFactory
import os

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

## Support

Here are the backend languages currently available :

- [X] python
- [ ] nodeJS
- [ ] golang
