# User

This document describe how users are defined into the chronos system.

## Base users

A user is able to connect to the application. By this way, this user is stored into the database with the following informations :
 * username : The user name
 * password : the user password
 * usertype : a descriminent to define if a user is administrator or simple user

### Password consideration

The password of a user must be encrypted to follow the minimum security consideration. Argon2 should be used as PHP7.2 is used.
