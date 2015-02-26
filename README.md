API
===

##User's API:
Hello users!

#### GET: api/problems
Get all moderated problems in brief (id, title, coordinates, type and status);

#### GET: api/problems/:id
get detailed problem description (all information from tables 'Problems', 'Activities', 'Photos') by it's id;

Request URL:
```
/api/problems/:id
```

where id is a number

Output information:

```
[
    [
        {
            "Id": 5,
            "Title": "Загрязнение Днепра",
            "Content": "В городе Берислав нет очистных сооружений.",
            "Proposal": "",
            "Severity": 3,
            "Moderation": 1,
            "Votes": 13,
            "Latitude": 46.8326,
            "Longtitude": 33.416462,
            "Status": 0,
            "ProblemTypes_Id": 4
        }
    ],
    [],
    [
        {
            "Id": 5,
            "Content": "{\"Content\":\"Проблему додано анонімно\",\"userName\":\"(Анонім)\"}",
            "Date": "2014-02-27T15:24:53.000Z",
            "ActivityTypes_Id": 1,
            "Users_Id": 2,
            "Problems_Id": 5
        }
    ]
]
```


#### GET API/users/:idUser
get user's name and surmane by id;

##### Expected request
~~~
/api/users/:idUser
~~~
:idUser === user id

##### Expected response
returns JSON with user's name and surname or empty JSON if there is no user with selected id
~~~~
{
    "json": [
        {
            "Name": "admin",
            "Surname": null
        }
    ],
    "length": 1
}
~~~~

#### GET /api/usersProblem/:idUser
Get all user's problems in brief (id, title, coordinates, type and status) by user's id;

##### Expected request
~~~
/api/usersProblem/id
~~~
where id is the user id

##### Expected response
returns array of user's problems and empty array if there is no user with such id
~~~~
[
    {
        "Id": 190,
        "Title": "назва3333",
        "Latitude": 51.419765,
        "Longtitude": 29.520264,
        "ProblemTypes_Id": 1,
        "Status": 0,
        "Date": "2015-02-24T14:27:22.000Z"
    }
]
~~~~

#### GET /api/activities/:idUser
get all user's activity (id, type, description and id of related problem);


----------------------

####POST /api/problempost

post new detailed environment problem to the server

#####Required headers
Content-Type: multipart/form-data; 


#####Request parameters:

| parameter | Description   |
|---------- |-------------- |
|title      | optional      |
|content    | optional      |
|proposal   | optional      |
|latitude   | optional      |
|longitude  | optional      |
|type       | 1-6, required |
|userId     | optional      |
|userName   | optional      |
|userSurname| optional      |

######Example Request:

http://localhost:8090/api/problempost

```
POST /api/problempost HTTP/1.1
Host: localhost:8090
Cache-Control: no-cache

----WebKitFormBoundaryE19zNvXGzXaLvS5C
Content-Disposition: form-data; name="title"

Врятувати ліс
----WebKitFormBoundaryE19zNvXGzXaLvS5C
Content-Disposition: form-data; name="content"

ліс страждає
----WebKitFormBoundaryE19zNvXGzXaLvS5C
Content-Disposition: form-data; name="proposal"

Позбавити страждань
----WebKitFormBoundaryE19zNvXGzXaLvS5C
Content-Disposition: form-data; name="latitude2"

45.4986464234233
----WebKitFormBoundaryE19zNvXGzXaLvS5C
Content-Disposition: form-data; name="longitude"

35.03564453125
----WebKitFormBoundaryE19zNbXGzXaLvS5C
Content-Disposition: form-data; name="type"
6

```

#####Response: 200
Content-type: application/json;charset=UTF-8

######Example 200 response:

```
{
    "json": {
        "fieldCount": 0,
        "affectedRows": 1,
        "insertId": 191,
        "serverStatus": 2,
        "warningCount": 0,
        "message": "\u0000",
        "protocol41": true,
        "changedRows": 0
    }
}
```
#####NOTE
The problem  will be seen pass in DB (api/problems) only after amdin confirmation 

**Server crash** if to parameter **'userId'** string char is passed;


#####Response: 500 Internal Server Error
if required parameter missed

######Example 500 response:
```
{
    "err": "ER_BAD_NULL_ERROR"
}
```
 
----------------------

#### POST /api/vote
vote for problem;

#### GET: /api/getTitles

Get title, id and alias of all resources;

Request URL:
```
/api/getTitles
```
Output information:

```
[
    {
        "Title": "Про проект",
        "Alias": "about",
        "Id": 1,
        "IsResource": 1
    },
    {
        "Title": "Як організувати прибирання в парку",
        "Alias": "cleaning",
        "Id": 2,
        "IsResource": 1
    },
    {
        "Title": "Як добитись ліквідації незаконного звалища?",
        "Alias": "removing",
        "Id": 3,
        "IsResource": 1
    },
    {
        "Title": "Як зупинити комерційну експлуатацію тварин?",
        "Alias": "stopping-exploitation",
        "Id": 4,
        "IsResource": 1
    },
    {
        "Title": "Торгують первоцвітами - телефонуй: \"102-187!\"",
        "Alias": "stopping-trade",
        "Id": 5,
        "IsResource": 1
    }
]

```

#### GET /api/resources/:name
get all information about resource by it's alias;

##### Expected request
~~~
/api/resources/:name
~~~
:name === one of about, cleaning, removing, stoping-exploitation, stoping-trade (resource name)

##### Expected response
returns JSON with description of resource
~~~~
[
    {
        "Id": 1,
        "Alias": "about",
        "Title": "Про проект",
        "Content": "  <p>Цей проект замислювався Всесвітнім фондом природи в Україні як платформа, \r\n  здатна об’єднати зусилля різних неурядових організацій, компаній та незалежних \r\n  активістів для обміну досвідом та покращення стану природи нашої країни.</p> \r\n\r\n  <p>Дуже часто про локальні проблеми екології знають тільки місцеві організації та \r\n  декілька сот жителів. А як було б добре підключити до їх вирішення однодумців \r\n  з інших частин країни!</p> \r\n\r\n  <p>За допомогою цього сайту будь-яка людина чи організація \r\n  може вказати на екологічну проблему свого регіону та вирішити її разом з усією \r\n  Україною!</p> \r\n\r\n  <p>Ви можете позначити екологічну проблему на карті, дізнатися та долучитися до \r\n  вирішення інших проблем у різних куточках України, або просто проголосувати \r\n  за проблему, якщо вважаєте її важливою та першочерговою.</p> \r\n\r\n  <p>До вирішення 5-ти найважливіших проблем (за результатами голосування) долучиться \r\n  безпосередньо команда WWF в Україні.</p> \r\n\r\n  <p>Також ми будемо підказувати Вам, що робити для вирішення Вашої проблеми. \r\n  Корисну інформацію з цього приводу Ви знайдете у розділі сайту «Ресурси», \r\n  який буде постійно поповнюватися. Якщо Вам необхідна додаткова інформація, \r\n  надішліть нам лист із запитом. Якщо у Вас є досвід, поділіться ним!</p>\r\n\r\n  <p>Практичну реалізацію проект отримав, як і всі добрі починання, силами активних \r\n  та небайдужих людей. Початковий варіант цього сайту був написаний <a href=\"http://symphony-solutions.eu\">3-ма людьми</a> \r\n  за 48 годин в межах доброчинного хакатону <a href=\"http://hack4good.io/\">Hack4Good</a>. </p>\r\n\r\n  <p>Сайт не є ідеальним, але ми майже щодня щось нове прикручуємо, щось фіксаємо та щось поліпшуємо. \r\n  Успіх цього проекту залежить від Вас, наших користувачів. Якщо Ви помітили помилку чи глюк – просимо не полінуватися і <a href=\"https://github.com/vredchenko/enviromap/issues?page=1&state=open\">зазвітувати тут</a> (New Issue)</p>\r\n\r\n  <p>Також нам потрібна Ваша посильна допомога. Долучайтесь!\r\n\r\n  <ul>\r\n    <li>Якщо Ви вмілий програміст і бажаєте допомагати в розвитку проекту – ми будемо раді бачити Вас в нашій команді;</li>\r\n    <li>Якщо Ви маєте зауваження чи побажання щодо розширення функціональності проекту;</li>\r\n    <li>Ми також шукаємо людей для наповнення інформаційної секції «Ресурси»;</li>\r\n    <li>На початковій стадії запуску цього сервісу кожний активний користувач для нас на вагу золота, тож люб&#8217;язно просимо нас лайкати і про нас щебетати.</li>\r\n  </ul>\r\n\r\n  Що більше людей та організацій долучаться, то більше ми зможемо зробити \r\n  для природи України!</p>\r\n\r\n  <h2>Контакти</h2>\r\n\r\n  <p>\r\n    <ul>\r\n      <li>Александра Ковбаско <a href=\"mailto:artimia@gmail.com\">artimia@gmail.com</a></li>\r\n      <li>Валерій Редченко <a href=\"mailto:lazyval@gmail.com\">lazyval@gmail.com</a></li>\r\n    </ul>\r\n  </p>\r\n",
        "IsResource": 1
    }
]
~~~~


#### GET /activities/:idUser
get user's activity list by user's id;

#### POST /api/photo/:id
add new photo to existing problem by problem's id;

##### Expected request
~~~
/api/photo/id
~~~
where id is the number of problem

##### Expected request body
~~~
------WebKitFormBoundaryP9TDO4Mn81TydWOh
Content-Disposition: form-data; name="userId"

1
------WebKitFormBoundaryP9TDO4Mn81TydWOh
Content-Disposition: form-data; name="userName"

admin
------WebKitFormBoundaryP9TDO4Mn81TydWOh
Content-Disposition: form-data; name="userSurname"


------WebKitFormBoundaryP9TDO4Mn81TydWOh
Content-Disposition: form-data; name="description"


------WebKitFormBoundaryP9TDO4Mn81TydWOh
Content-Disposition: form-data; name="null"

Додати
------WebKitFormBoundaryP9TDO4Mn81TydWOh
Content-Disposition: form-data; name="null"

Звернути
------WebKitFormBoundaryP9TDO4Mn81TydWOh
Content-Disposition: form-data; name="file[0]"; filename="Desert.jpg"
Content-Type: image/jpeg
~~~
##### Expected response
~~~
{
    "json": [],
    "length": 0
}
~~~

#### POST /api/comment/:id
add new comment to problem by problem's id;


----------------------

####POST /api/login

log in for user (email and password are required). User's id, name, surname, role and secret token will be returned;

#####Required headers:
Content-Type: application/json;charset=utf-8

#####Request parameters:

|parameter | Description |
|--------- |-------------| 
|email     | required    |
|password  | required    |

######Example Request:

http://localhost:8090/api/login

```
  {
    "email":"vasia@gmail.com",
    "password":"vasia"
  }
```
#####Response: 200
if OK
Content-type: application/json;charset=UTF-8

######Example 200 response:

```
  {
    "id":92,
    "name":"Вася",
    "surname":"Вася",
    "role":"user",
    "iat": 3445678654,
    "token":"tjJ0eSAiOiJKV1QiLCJhbGciPpJIUzI1NiJ9.eyJpZCI6MSwibmFtZSI6ImFkbWluIiwic3VybmFtZSI6bnVsbCwicm9sZSI6ImFkbWliaXN0cmF0b3IiLCJpYXQiOjE0MjQ3OTExNTN9.Bu2ihpRoJ0vF5af473HgtYxKQE_knKkuM3f8TLVjUEw",
    "email":"vasia@gmail.com"
  }
```

#####Response: 401 Unauthorized
if email or password is wrong

######Example 401response:

```
Unauthorized
```


####Response: 400 Bad Request
syntax error (if email or password is wrong)

#####Example 401response:

```
Bad Request
```
----------------------

#### GET /api/logout
log out; 

#### POST: /api/register

Register (name, surname, email, password are required). User's id, name, surname, role and secret token will be returned;

Headers:

|   Header   |              Value             |
| ---------- |:------------------------------:|
|Content-Type| application/json;charset=UTF-8 |


Request URL:
```
api/register
```

Request body:

```
{"first_name":"Name","last_name":"Surname","email":"your_email@mail.com","password":"SmThRea11yStr0nG"}
```

Output information:
```
{
    "name": "Name",
    "surname": "Surname",
    "userRoles_Id": 2,
    "id": {
        "fieldCount": 0,
        "affectedRows": 1,
        "insertId": 12,
        "serverStatus": 2,
        "warningCount": 0,
        "message": "",
        "protocol41": true,
        "changedRows": 0
    },
    "role": "user",
    "iat": 1424711155,
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiRGFzaGEiLCJzdXJuYW1lIjoiRGkiLCJ1c2VyUm9sZXNfSWQiOjIsImlkIjp7ImZpZWxkQ291bnQiOjAsImFmZmVjdGVkUm93cyI6MSwiaW5zZXJ0SWQiOjEyLCJzZXJ2ZXJTdGF0dXMiOjIsIndhcm5pbmdDb3VudCI6MCwibWVzc2FnZSI6IiIsInByb3RvY29sNDEiOnRydWUsImNoYW5nZWRSb3dzIjowfSwicm9sZSI6InVzZXIiLCJpYXQiOjE0MjQ3MTExNTV9.4Uz0k8SYc71LfALdvJtBdcqco0I6hg-Ses-fknrF6WY"
}
```

Status codes:

**400** - Bad Request - if user has already existed;

**401** - Unauthorized - if one of the field is incorrect or empty.

#### POST /api/changePassword

Changes password

Headers:

|   Header   |              Value             |
| ---------- |:------------------------------:|
|Content-Type| application/json;charset=UTF-8 |


##### Expected request
```
api/changePassword
```

Request body:

```
{"userId":"1", "old_password": "admin", "new_password":"admin", "new_password_second":"admin"}
```

##### Expected response
```
{
    "result": "success"
}
```

Status codes:

**401** - Unauthorized - if you aren't authorized or one of request fields are empty



Admin's API:
------------

#### GET /api/not_approved
get all problems which are not approved in brief (id, title, coordinates, date of creation);

----------------------

####DELETE /api/problem/:id

delete single problem from database by if (all information from tables 'Problems', 'Activities', 'Photos')

#####Method: DELETE

#####Request:
api/problem/:id


#####Required headers:
Content-type: application/json; charset=utf-8
Authorization: (Cookie)

######Example Request:
```
http://localhost:8090/api/problem/101
```

#####Response: 200
Content-type: application/json;charset=UTF-8

######Example response:

```
{
    "result": "success",
    "err": "",
    "json": {
        "fieldCount": 0,
        "affectedRows": 1,
        "insertId": 0,
        "serverStatus": 2,
        "warningCount": 0,
        "message": "\u0000",
        "protocol41": true,
        "changedRows": 0
    }
}
```

#####Response: 401 Unauthorized
if user don't authorized (no token in cookie)

######Example 401 response:
```

  Unauthorized

```
----------------------

#### DELETE /api/user/:id
Delete user by user's id (only from 'Users');

#### DELETE: api/activity/:id

Delete comment by it's id;


Headers:

|   Header   |              Value             |
| ---------- |:------------------------------:|
|Content-Type| application/json;charset=UTF-8 |
|   Cookie   |     token=//admin's token//    |

Request URL:
```
/api/activity/:id
```
where id  is a number

Output information:
```
{
    "result": "success"
}
```
**401** - Unauthorized - if you are not admin

#### DELETE api/photo/:id
delete photo by photo's id;

#### PUT /api/edit/:id
edit problem (update all fields) by it's id;

##### Expected request
~~~
/api/editProblem/id
~~~
where id is the number of problem

##### Expected request body
~~~
{"Title":"string","Content":"string","Proposal":null,"Severity":2,"ProblemStatus":true}
~~~
##### Expected response
~~~
{
    "result": "success",
    "err": "",
    "json": {
        "fieldCount": 0,
        "affectedRows": 1,
        "insertId": 0,
        "serverStatus": 2,
        "warningCount": 0,
        "message": "(Rows matched: 1  Changed: 0  Warnings: 0",
        "protocol41": true,
        "changedRows": 0
    }
}
~~~
##### Error codes

401 - Unauthorized - if not logged in as admin

#### POST api/approve/:id

Approve problem by it's id;

Headers:

|   Header   |              Value             |
| ---------- |:------------------------------:|
|Content-Type| application/json;charset=UTF-8 |
|   Cookie   |     token=//admin's token//    |

Request URL:
```
/api/approve/:id
```
where id  is a number

Output information:
```
{
    "result": "success",
    "err": "",
    "json": {
        "fieldCount": 0,
        "affectedRows": 1,
        "insertId": 0,
        "serverStatus": 2,
        "warningCount": 0,
        "message": "(Rows matched: 1  Changed: 0  Warnings: 0",
        "protocol41": true,
        "changedRows": 0
    }
}
```
**401** - Unauthorized - if you are not admin

#### DELETE /api/photo/:id
delete photo by photo's id;

#### PUT /api/edit/:id
edit problem (update all fields) by it's id;

#### POST /api/addResource
Add new resource into header;

----------------------

####PUT /api/editResource/:id

#####Required headers:
Content-type: application/json; charset=utf-8
Authorization (cookie) aren't required(!);

#####Request parameters:

| parameter | Description   |
|---------- |-------------- |
|Alias      | optional      |
|Content    | optional      |
|Title      | optional      |
|IsResource | optional      |

Title could be tricky. If it dosn't specify resource with specified ID will be deleted;
The same result occered then no JSON object has sent with request.

######Example Request:
http://localhost:8090/api/editResource/1

```
{
    "Alias":"testing",
    "Content":"<p>Some text</p>",
    "Title":"TestTest",
    "IsResource":1
}
```

#####Response: 200
Type: JSON

######Example response:

```
{
    "result":"success",
    "err":""
}
```

----------------------

#### DELETE /api/deleteResource/:id
delete resource by it's id;

#### POST /api/postNews
add message to the newsline;

#### GET /api/getNews
get all messages for newsline;

##### Expected request
~~~
/api/getNews
~~~

##### Expected response
return json with existing news
~~~
{
    "news": [
        {
            "Id": 1,
            "Content": "fgfgfgfgfg"
        },
        {
            "Id": 2,
            "Content": "sdfdsfdsf"
        }
    ]
}
~~~

#### POST /api/clearNews
delete all messages from newsline;

----------------------

####POST  /api/clearOneNews
delete one message from newsline;

######Required headers:
Content-type: application/json; charset=utf-8
Authorization (cookie);

#####Request parameters:


| parameter | Description   |
|---------- |-------------- |
|content    | optional      |

######Example Request:
http://localhost:8090/api/clearOneNews

```
{
    "content":"ddfdfs"
}
```

#####Response: 200
Content-type: application/json;charset=UTF-8

######Example response:

```
{
"news": { 
        "fieldCount":0,
        "affectedRows":1,
        "insertId":0,
        "serverStatus":34,
        "warningCount":0,
        "message":"",
        "protocol41":true,
        "changedRows":0
        }
}
```

----------------------

#### GET: api/getStat2/:val

Number of added problems for value:

| Value | Means        |
| ----- |:------------:|
|   D   | the last day |
|   W   | week         |
|   M   | month        |
|   Y   | year         |
|   A   | All Time     |

Headers:

|   Header   |              Value             |
| ---------- |:------------------------------:|
|Content-Type| application/json;charset=UTF-8 |
|   Cookie   |     token=//admin's token//    |

Request URL:
```
/api/getStats2/A
```


Output information:
```
[
    {
        "id": 1,
        "value": 25
    },
    {
        "id": 2,
        "value": 65
    },
    {
        "id": 3,
        "value": 7
    },
    {
        "id": 4,
        "value": 41
    },
    {
        "id": 5,
        "value": 10
    },
    {
        "id": 6,
        "value": 9
    },
    {
        "id": 7,
        "value": 31
    }
]
```
where id refers to the problem's name

#### GET: /api/getStats4
get all statistic problems

##### Expected request
~~~
/api/getStats4
~~~
##### Expected response
return json with 3 arrays of problems
~~~
0: [{Id: 54, Title: "Завод з токсичними викидами в житловій зоні", Votes: 48},…]
1: [{Id: 11, Title: "Місцевість дуже забруднена, місто потерпає від сміття", Severity: 3}, {Id: 10,…},…]
2: []
~~~
