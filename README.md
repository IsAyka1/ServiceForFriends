# ServiceForFriends
`django` `rest frameword` `social network` `server` `postgres`

Django-service for friends


## Server can do
- register a new user
- send friend-request from user to another user
- accept/deny friend-request from another user
- show list of user's friends
- show list of incoming friends-requests
- show list of outgoing friends-requests
- check status of relationship between users
  - nothing
  - incoming request
  - outgoing request
  - friendship
- delete user from another user's list of friends (request won't be created)
- auto accept requests if user1 send to user2 and user2 send to user1


## DataBase 
Based on Postgres 13.3

### Model 'User'
Table of user's informations. It can be email/age/birthday and e.t.c
- id: uuid
- name: str

### Model 'Relation'
Table of relations between users if it's requested to be friends or friends already
- from_user: User
- to_user: User
- relation: Enum
  - Friend
  - Request

# How to use

* If you want start working with full db, run:
```commandline
pip install requests
python download_pgdata.py
docker-compose up
```
* Else if you want empty db, run
```commandline
docker-compose up -d
docker_compose exec web python manage.py createsuperuser --username=admin --email=test@email.com --no-input
```

Wait for loading and then

You can go to 0.0.0.0:8000 (0.0.0.0:8000/swagger) and check rest api

**Read ```server_for_friend/swagger.yaml```**

## Example

* Get list of all users

  - 
      ```commandline
      curl -X 'GET' 
      'http://0.0.0.0:8000/users/' 
      -H 'accept: application/json' 
      ```
  - response 200
      ```python
      [
       {
           "id": "1",
           "name": "Ayka"
       },
       {
           "id": "2",
           "name": "Mike"
       },
      ]
     ```

* Create new user (registration)

  - 
      ```commandline
      curl -X 'POST' 
      'http://0.0.0.0:8000/users/' 
      -H 'accept: application/json' 
      -H 'Content-Type: application/json' 
      -d '{
          "name": "ayka"
      }'
      ```
  - response 200
      ```python
       {
           "id": "1",
           "name": "Ayka"
       }
     ```
    
* Get all friends of user by user_id

  - 
      ```commandline
     curl -X 'GET' 
    'http://0.0.0.0:8000/users/' 
    -H 'accept: application/json' 
    -H 'id: 1'
    ```
  - response 200
     ```python
      [
         {
           "id": "2",
           "name": "Mike"
         },
      ]
    ```
* Get list of all relations

  -  
     ```commandline
     curl -X 'GET' 
     'http://0.0.0.0:8000/relation/' 
     -H 'accept: application/json' 
     -H 'id: 1'
    ```
  - response 200
    ```python
      [
         {
           "from_user": "1",
           "to_user": "2",
           "realtion": "F"
         },
      ]
    ```

* Send request

  -
     ```commandline
      curl -X 'POST' 
      'http://0.0.0.0:8000/relation/send_request/' 
      -H 'accept: application/json' 
      -H 'id: 1' 
      -H 'Content-Type: application/json' 
      -d '{
      "to_id": "3"
      }'
     ```
  - response 200
     ```python
       {
         "from_user": "1",
         "to_user": "3",
         "realtion": "R"
       }
     ```  


## How to admin
Go to ```0.0.0.0:8000/admin```

To login check environment file ```.env``` with `DJANGO_SUPERUSER_USERNAME`, `DJANGO_SUPERUSER_PASSWORD`.
You can:
- add User (add relation with another existing user)
- add Relation between existing users 
(Attention: if you create user1->user2 and user2->user1 they won't be friends)
- see all of Users
- see all of Relations
- delete User
- delete Relation
- update User's fields:
  - name
- update Relation's fields:
  - relation

[//]: # (# to test)

[//]: # ()
[//]: # (```commandline)

[//]: # (python manage.py test)

[//]: # (```)
