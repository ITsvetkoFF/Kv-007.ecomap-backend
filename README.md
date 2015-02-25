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


+ app.get('/users/:idUser', routes.getUserId) - get user's name and surmane by id;

+ app.get('/api/usersProblem/:idUser', routes.getUserProblemsById) - get all user's problems in brief (id, title, coordinates, type and status) by user's id;

+ app.get('/api/activities/:idUser', routes.getUserActivity) - get all user's activity (id, type, description and id of related problem);

+ app.post('/api/problempost', routes.postProblem) - post new problem;

+ app.post('/api/vote', routes.postVote) - vote for problem;

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

+ app.get('/api/resources/:name',routes.getResource) -get all information about resource by it's id;

+ app.get('/activities/:idUser', routes.getUserActivity) - get user's activity list by user's id;

+ app.post('/api/photo/:id',routes.addNewPhotos) - add new photo to existing problem by problem's id;

+ app.post('/api/comment/:id',routes.addComment) - add new comment to problem by problem's id;

+ app.post('/api/login', routes.logIn) - log in (email and password are required). User's id, name, surname, role and secret token will be returned;

+ app.get('/api/logout', routes.logOut) - log out; 

#### POST: api/register

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


Admin's API:
------------

+ app.get('/api/not_approved', routes.notApprovedProblems) - get all problems which are not approved in brief (id, title, coordinates, date of creation);

+ app.delete('/api/problem/:id', routes.deleteProblem) - delete problem by it's id (all information from tables 'Problems', 'Activities', 'Photos');

+ app.delete('/api/user/:id', routes.deleteUser) - delete user by user's id (only from 'Users');

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

+ app.delete('/api/photo/:id', routes.deletePhoto) - delete photo by photo's id;

+ app.put('/api/edit/:id', routes.editProblem) - edit problem (update all fields) by it's id;

#### POST: api/approve/:id

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

+ app.delete('/api/photo/:id', routes.deletePhoto) - delete photo by photo's id;

+ app.put('/api/edit/:id', routes.editProblem) - edit problem (update all fields) by it's id;

+ app.post('/api/addResource', routes.addResource) - add new resource into header;

+ app.put('/api/editResource/:id', routes.editResource) - edit existing resource;

+ app.delete('/api/deleteResource/:id', routes.deleteResource) - delete resource by it's id;

+ app.post('/api/postNews',routes.postNews) - add message to the newsline;

+ app.post('/api/getNews',routes.getNews) - get all messages for newsline;

+ app.post('/api/clearNews',routes.clearNews) - delete all messages from newsline;

+ app.post('/api/clearOneNews',routes.clearOneNews) - delete one message from newsline;

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

