Simple website

```Python
from bottle import *

@route('/')
def  welcome():
  response.content_type = 'text/plain'
  return 'Hello'
  
if __name__== '__main__':
  run(host='localhost', port=8080)
  
```
