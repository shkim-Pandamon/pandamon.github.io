---
title: "[KEPCO] Constructing Demo System for AI based Diagnosis_6"
excerpt: "Let's build up a demo server of KEPCO SEDA system"

categories:
    - Blog
tages:
    - Blogs
last_modified_at: 2020-04-03T12:24:00
---

## Goal
> To construct `Python Web Framework` using `Flask` and get the result on `Web` using `RestAPI`   
> <small>*Flask를 이용해서 파이썬 웹프레임워크를 구축하고 RestAPI를 이용해 결과값을 웹이서 받아보자*</small>

## System Specification
This project uses following HW resources.   
<small>*본 프로젝트에 사용된 HW 리소스는 다음과 같습니다.*</small>  

> - Oracle Server: Linux (`Ubuntu 20.04`)
> - AI Server: Linux (`Ubuntu 18.04`)  <span style="color:red">(Today's Use)</span>
> - Client Server: macOS (`Big sur`) on M1 chip

## Web Framework?
`Web framework` is a framework for web service development. Then what is `Framework`? `Framework` is a set of various classes and libraries which are for a specific objective. Thus, `Web framework` is a set of classes and libraries collected for web service development. Roughly explained with our project, it plays a role to transfer the result of algorithm to the web.  
<small>*먼저 웹프레임워크에 대한 개념부터 간단히 설명하고 넘어가도록 하겠습니다. 웹 프레임워크는 웹 서비스 개발을 위한 '프레임워크'입니다. 그렇다면 프레임워크란 무엇일까요? 프레임워크는 다양한 클래스와 라이브러리를 묶어둔 덩어리로 볼 수 있습니다. 정리하면 웸 프레임워크란 웹 서비스 개발을 위한 다양한 클래스와 라이브러리들을 묶어둔 코드들의 집합이라고 할 수 있습니다. 저희 문제에 빗대어 쉽게 표현해주면 우리의 파이썬 알고리즘이 만들어낸 결과값을 웹에 던져주는 역할을 맡고 있습니다.*</small>

There are two very famous web frameworks for `Python`: `Django` and `Flask` respectively. I would skip the details of differences of two, since there are lots of posts that explain it. Simply and roughly, `Django` is a full-stacked framework which is more complicate and deeper, and `Flask` is a micro framework which is lighter and more simple. In our project, we adopt `Flask` as a web framework owing to its easy management and expandability, thus, in this post, I am gonna explain how to construct web framework using `Flask`.   
<small>*파이썬에는 두 유명한 웹프레임워크가 있는데요, 각각 'Django'와 'Flask'입니다. 이 둘을 비교하는 설명은 다른 블로그에 많이 있고, 저희는 실사용을 주 목적으로 하므로 깊은 개념은 넘어가도록 하겠습니다. 간단히 요약하면 'Django'는 보다 복잡하지만 탄탄하고 깊이가 깊은 Full-stack 프레임워크이고, 'Flask'는 보다 간단하고 가벼운 Micro 프레임워크입니다. 저희 프로젝트에서는 확장성이나 쉬운 관리를 위해 'Flask'를 사용하고 있으며, 본 포스트에서도 'Flask'를 통한 구현을 설명하도록 하겠습니다.*</small>

## What is RestAPI?
Next, Let's see what `RestAPI` is. Even though you are not majoring programming, you might frequently encounter an keyword such as `RestAPI` or `API`. `RestAPI` is a integration of `Restful API`. First, `API` is an acronym of `Application Programming Interface`, and it is a `specification` defining some protocols and sub-programs for the communiation between programs. Roughly speaking, it is a kind of statement that 'If you request `AA` data with the form of `BB`, then I will offer you the data with the form of `CC`'.  
<small>*다음으로 RestAPI에 대해 알아보도록 하겠습니다. 비전공자분들도 'RestAPI'라는 키워드를, 특히 'API'라는 키워드를 종종 들어보셨을 겁니다. 특히 '요즘 웹서비스는 대부분 RestAPI형식으로 제공해'와 같은 말들을 종종 들어보셨을 거에요. (혹은 처음 들어보셨다해도 전혀 문제되지 않습니다.) RestAPI는 Rest한 API를 지칭합니다. API란 무엇일까요? API는 Application Programming Interface의 약자로, 쉽게 말해 프로그램간 소통하기 위해 프로토콜, 부프로그램등의 정의한 '사양'을 말합니다. 아주 거칠게 말해서 '네가 데이터를 AA형식으로 요청하면 나는 BB데이터를 CC형식으로 제공할게'라는 성명입니다. 그리고 최근 OpenAPI처럼 불리는 것들은 이 성명을 프로그램화 하여 유저들이 쉽게 요청할 수 있도록 구현해둔 것을 말합니다.*</small>

Then, what does `Rest` mean? `Rest` is *Representational State Transfer*. In my case, understanding the compositions of `Rest API` was an easier step to understand the entire concept. The three compositions of it are as follow:  
<small>*그러면 'Rest'하다는 것은 무엇일까요? 'Rest'는 Representational State Transfer의 준말입니다. 직관적으로 이해되지 않을 수도 있는데요, 개념보다는 구성을 먼저 살펴보는 것이 이해가 편할 수 있습니다. Rest는 다음의 세가지로 구성되어 있습니다.*</small>

> - Resource: `URI`  
> - Verb: `HTTP` Method (`GET`, `POST`, `PUT`, `DELETE`)  
> - Representaion: Data with the form of `JSON`, `XML`, `TEXT`, `RSS`, and etc.,  

Now, Let's understand the concept of `Rest API`. `Rest API` points the resource with `URI`(Uniform Resource Identifier), and operate CRUD (Create, Read, Update, Delete) actions using `4 HTTP Methods` with the data form in `JSON`, `XML`, `TEXT`, `RSS`, and etc. There are some rules and characteristics of `Rest API`, but I am not cover the details. Anyway, back to our project, the thing that we gonna use `Rest API` in our project means follow:  
<small>*자, 이제 다시 'Rest API'의 개념을 살펴봅시다. 'Rest API'는 URI(Uniform Resource Identifier)를 통해 자원(Resource)를 포인팅하고, HTTP의 4개 메서드(POST, GET, PUT, DELETE)를 이용해서 자원에 대한 CRUD(Create, Read, Update, Delete) Operation을 수행하는 것을 말합니다. 이를 위한 몇가지 규칙과 특징들이 존재하는데요, 이에 대한 구체적인 내용은 여기서 다루지 않도록 하겠습니다. 어쨌든 이제 본론으로 돌아와서 우리 프로젝트에 Rest API를 쓰겠다는 것은 다음을 의미합니다:*</small>

> 1. We would make applications.
> 2. We would allocate `URIs` on applications.
> 3. We would do CRUD operation using `HTTP methods`.
> 4. We would take `JSON` format for data.

## Build Application
First, Let's make an easy example application. This application call all data from 'test table' on `Oracle DB`, as previous post. But in this case, I would modify the script as a from of `Class` to make it as application.  
<small>*자 먼저 쉬운 예제 어플리케이션을 만들어보도록 하겠습니다. 이 어플리케이션은 이전에 구현했던 것처럼 오라클 DB의 'test_table'이라는 테이블에서 모든 데이터를 호출하는 어플리케이션입니다. 다만 이를 어플리케이션화 하기 위해 스크립트를 Class로 변형하도록 하겠습니다.*</small>

```python
# Bellow will be saved with the name of 'application_example.py'
import cx_Oracle
LOCATION = '/opt/oracle/instantclient_19_9'
cx_Oracle.init_oracle_client(lib_dir=LOCATION)

class ExampleApplication(object):
    def __init__(self):
        # set the configuration of your oracle dB
        self.host = cx_Oracle.makedsn(<address>, <port>, <SID>) # New D
        self.id = <id>
        self.pw = <password>

    def run(self):
        # make a query.
        query = 'select * from test_table'

        # connet to Oracle DB
        conn = cx_Oracle.connect(self.id, self.pw, self.host)
        cur = conn.cursor()
        cur.execute(query)
        cur.rowfactory = self.makeDicFactory(cur)
        result = []
        for row in cur.fetchall():
            result.append(row)
        cur.close()
        conn.close()
        return result

        # convert data to dictionary
    def makeDicFactory(self, cur):
        columnNames = [d[0] for d in cur.description]
        def createRow(*args):
            return dict(zip(columnNames, args))
        return createRow
```

## Make API
Then, Let's make API. Since we will make `Rest API` using `Flask`, we would use `Resource` of `flask_restful`. In addition, we would add some tools to track an occurence of error.  
<small>*이번에는 API를 만들어보도록 하겠습니다. 우리는 flask를 이용해 RestAPI를 만들 예정이므로 Flask_restul의 Resource를 사용할 예정입니다. 여기에 몇가지 에러 발생을 Tracking하기 위한 장치도 함께 구성해주도록 합니다.*</small>

```python
# Bellow will be saved with the name of 'api_example.py'
from flask_restful import Resource
from api.errors import SchemaValidationError, InvalidTimestampFormatError, InvalidIntegerFormatError, TooManyRequestsTypeAError, TooManyRequestsTypeBError, InvalidParametersError
import logging


class ApplicationAPI(Resource):
    @classmethod
    def get(cls): # HTTP Method is GET
        try: # try running our application
            from application_example import ExampleApplication
            result = ExampleApplication().run()
            return result
        
        # Exception for Invalid Timestamp Format
        except InvalidTimestampFormatError:
            raise InvalidTimestampFormatError

        # Exception for the error on application
        except Exception as ex:
            logger = logging.getLogger('flask_app')
            logger.error(ex)
            template = "An exception of type {0} occurred. Arguments:\n{1!r}"
            message = template.format(type(ex).__name__, ex.args)
            return {
                'status': 500,
                'message': message
            }, 500
```
## Run Application
Finally, Let's run the server.  
<small>*이제 아래와 같이 서버를 실행해봅시다.*</small>

```python
from flask import Flask, Blueprint
import flask_restful
import flask_cors

def create_app(): 
    _app = Flask("flask_app")

    # register to pre-handle request
    @_app.before_request
    def pre_request():
        import threading
        import uuid
        thread = threading.current_thread()
        thread.request_id = str(uuid.uuid4())
        
    with _app.app_context():        
        from libs.logger import Logger, info
        Logger(app=_app, logger_name="flask_app")

        # REST API module
        from api_example import ApplicationAPI

        # handle Exception module
        from api.errors import errors

        ## Adding Resource        
        flask_cors.CORS(_app, resources={r"/*": {"origins": "*"}})                
        api_blueprint = Blueprint('demo_system', __name__, url_prefix='/{prefix}/v{version}'.format(prefix='api', version='1'))
        rest_api = flask_restful.Api(api_blueprint, errors=errors)
        rest_api.add_resource(ApplicationAPI, '/application')
        _app.register_blueprint(api_blueprint)        
    return _app

app = create_app()

if __name__ == '__main__': # pragma: no cover    
    app.run(host='0.0.0.0', port=8000, use_reloader=False, use_debugger=False, threaded=True)
```

If it works properly, you can request with the [http://localhost:8000/api/v1/application.](http://localhost:8000/api/v1/application)   
<small>*지금까지 문제없이 따라왔다면 URL 'http://localhost:8000/api/v1/application'를 요청해봅시다.*</small>
