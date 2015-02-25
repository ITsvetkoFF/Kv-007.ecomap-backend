API
===
User's API:
-----------

+ app.get('/problems', routes.getProblems) - get all moderated problems in brief (id, title, coordinates, type and status);

+ app.get('/problems/:id', routes.getProblemId) - get detailed problem description (all information from tables 'Problems', 'Activities', 'Photos') by it's id;

+ app.get('/users/:idUser', routes.getUserId) - get user's name and surmane by id;

#### GET: /api/usersProblem/:idUser
Get all user's problems in brief (id, title, coordinates, type and status) by user's id

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

+ app.get('/api/activities/:idUser', routes.getUserActivity) - get all user's activity (id, type, description and id of related problem);

+ app.post('/api/problempost', routes.postProblem) - post new problem;

+ app.post('/api/vote', routes.postVote) - vote for problem;

+ app.get('/api/getTitles',routes.getTitles) - get title, id and alias of all resources;

+ app.get('/api/resources/:name',routes.getResource) -get all information about resource by it's id;

+ app.get('/activities/:idUser', routes.getUserActivity) - get user's activity list by user's id;

#### POST: /api/photo/:id
add new photo to existing problem by problem's id

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
+ app.post('/api/comment/:id',routes.addComment) - add new comment to problem by problem's id;

+ app.post('/api/login', routes.logIn) - log in (email and password are required). User's id, name, surname, role and secret token will be returned;

+ app.get('/api/logout', routes.logOut) - log out; 

+ app.post('/api/register', routes.register) - register (name, surname, email, password are required). User's id, name, surname, role and secret token will be returned;

Admin's API:
------------

+ app.get('/api/not_approved', routes.notApprovedProblems) - get all problems which are not approved in brief (id, title, coordinates, date of creation);

+ app.delete('/api/problem/:id', routes.deleteProblem) - delete problem by it's id (all information from tables 'Problems', 'Activities', 'Photos');

+ app.delete('/api/user/:id', routes.deleteUser) - delete user by user's id (only from 'Users');

+ app.delete('/api/activity/:id', routes.deleteComment) - delete comment by it's id;

+ app.delete('/api/photo/:id', routes.deletePhoto) - delete photo by photo's id;

#### PUT: /api/edit/:id
edit problem (update all fields) by it's id

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

+ app.post('/api/addResource', routes.addResource) - add new resource into header;

+ app.put('/api/editResource/:id', routes.editResource) - edit existing resource;

+ app.delete('/api/deleteResource/:id', routes.deleteResource) - delete resource by it's id;

+ app.post('/api/postNews',routes.postNews) - add message to the newsline;

#### GET: /api/getNews
get all messages for newsline

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
+ app.post('/api/clearNews',routes.clearNews) - delete all messages from newsline;

+ app.post('/api/clearOneNews',routes.clearOneNews) - delete one message from newsline;

+ app.get('/api/getStats1', routes.getStats1);

+ app.get('/api/getStats2/:val', routes.getStats2);

+ app.get('/api/getStats3', routes.getStats3);

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
