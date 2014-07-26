###Note:###
Gondalf is under heavy development which might lead to changes in request or/and response formats. 

##What is Gondalf?##
Gondalf is a ready to deploy microservice that provides user management, authentication, and role based authorization features out of box. Gondalf is built using [martini](https://github.com/go-martini/martini) and [gorm](https://github.com/jinzhu/gorm), and uses [postgresql](http://www.postgresql.org) as the default database.

##Features:##

###1. User management###
- User creation
- Validating unique username
- Password change on first login
- Encrypted password storage
- Activity logs

###2. Authentication###
- User authentication
- Token-based authentication
- Custom token expiry and renewal times

###3. Authorization###
- Role-based authorization including group permissions


##Why Gondalf?##
Over the course of multiple projects I realized that there are some common features that can be packed into a single microservice and can be used right out of the box. Gondalf is the first piece in that set.


##TODO List##
- [ ] Add end points for permission checking
- [ ] Improve documentation 
- [ ] Add more events to Activity Logs
- [ ] Input timeout values and server port from config file
- [ ] Add SSL support for the end point
- [ ] Add CI on checkins
- [ ] Dockerize gondalf
- [ ] Provide one click deploy solution
- [ ] Add support for other databases


###Why call it *Gondalf* ?###

Because  <img src="http://www.reactiongifs.com/wp-content/uploads/2013/12/shall-not-pass.gif" width="100px" height="50px"/> and it is Go, so why not both?


 
##Installation Instructions:##

- Clone the repository in a local directory

- Database configuration can be set under the the config file named gondalf.config

- Gondalf creates required tables using the configuration provided in the config file. For this the 
initdb flag should be set to true when starting the app.

`$ bash startApp.sh -initdb=true` 

##Request and Response formats##

###Error Codes###

- Invalid Session Token
- Expired Session Token
- Unregistered User
- Invalid Password
- First Login Change Password
- Authentication Failed
- Encryption Failed
- Database Error
- Permission Denied


###LoginCredential###

####Request####

```javascript
{
  "username": "test2User",
  "password" : "testPassword",
  "deviceId" : 1
}
```

deviceId code 1 for web, 2 for mobile

####Response####

```javascript
{
  "sessionToken": "testSessionToken",
  "error":"errorResponse"
}
```

###ValidateUsername###

For validating unique username

####Request####

```javascript
{
	"username": "test2User"
}
```

####Response####

```javascript
{
	"valid": true/false
}
```

###CreateUser###

####Request####

```javascript
{
	"username": "test2User",
	"legalname": "testLegalName",
	"password": "testPassword"
}
```
####Response####

```javascript
{
	"userCreated": true/false
}
```

###Change password###

####Request####

```javascript
{
	"username": "test2User",
	"oldPassword": "testOldPassword",
	"newPassword": "newTestPassword",
	"deviceId": 1
}
```

####Response####

If the old credentials are correct then:

```javascript
{
	"passwordChanged": true,
	"sessionToken": "testSessionToken",
	"error": nil
}
```

else

```javascript
{
	"passwordChanged" :false
}
```

###Validate Session Token###

####Request####

```javascript
{
	"sessionToken": "testSessionToken"
}
```

####Response####

Returns userId = -1 if there is an error

```javascript
{
	"userId": 1234,
	"error": nil
}
```