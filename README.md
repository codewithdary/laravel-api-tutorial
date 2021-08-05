## Laravel API Tutorial

The following documentation is based on my  Laravel 8 API [Tutorial for Beginners]( https://www.youtube.com/watch?v=xvqPEEpRBJ4) where I’ll show you how to work with API’s in Laravel.
•	Author: Code With Dary <br>
•	Twitter: [@codewithdary](https://twitter.com/codewithdary) <br>
•	Instagram: [@codewithdary](https://www.instagram.com/codewithdary/) <br>

## Requirements
•	Laravel Collections <br>
•	Basics of PHP <br>
•	Basics of Laravel <br>

## Topics
•	Introduction to API’s
•	HTTP Methods & Responses
•	JSON Api Best Practices
•	How to setup Laravel Passport
•	How to create access tokens
•	How to build resources
•	CRUD REST API with Laravel

## Usage <br>
Setup the repository <br>
```
git clone git@github.com:codewithdary/laravel-collections.git
cd bookstore
Composer install
cp .env.example .env 
php artisan key:generate
php artisan cache:clear && php artisan config:clear 
php artisan serve 
```

## Database Setup <br>
We are going to pull in data from the database so make sure that you have your database setup
```
mysql;
create database bookstore_collections;
exit;
```


Setup your database credentials in the .env file <br>
```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=cars
DB_USERNAME={USERNAME}
DB_PASSWORD={PASSWORD}
```

Migrate the tables
```
php artisan migrate
```	
