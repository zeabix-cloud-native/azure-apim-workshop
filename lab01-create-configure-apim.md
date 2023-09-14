# Lab-01 - Create and Configure API in APIM to associate with backend APIs


## Exercise 1: Explore backend service API
Zeabix has been prepared the backend service API for the workshop, let's explore the backend APIs which will be used in `Lab-01` for backend service

### Create User Profile API
`API Endpoint: https://tbd.com/profile/v1/profiles`

`Method: POST`

`Request Body`
```
{
    "username": "<string>",
    "firstname": "<string>",
    "lastname": "<string>",
    "avatar": "<string>"
}
```

#### Test Create User Profile API
`$ curl -XPOST https://tbd.com/profile/v1/profiles -H 'Content-type: application/json' -d '{"username": "yourusername", "firstname": "YourFirstname", "lastname": "YourLastname", "avatar": "https://placeholder.com/sample.png"}'`

NOTE: You can also use the `Postman` to call this API

### List All User Profile API
`API Endpoint: https://tbd.com/profile/v1/profiles`

`Method: GET`

#### Test list all user profiles
`$ curl https://tbd.com/profile/v1/profiles`


### Get User by ID API
`API Endpoint: https://tbd.com/profile/v1/profiles/{id}`

`Method: GET`

#### Test get user profile by ID
`$ curl https://tbd.com/profile/v1/profiles/<your-id>`

## Exercise 2: Manual Create API (HTTP)



### Testing by calling API through APIM


## Exercise 3: Create API by importing OpenAPI specification