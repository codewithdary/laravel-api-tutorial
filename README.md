## Laravel API Tutorial

The following documentation is based on my  [Laravel 8 API Tutorial for Beginners]( https://www.youtube.com/watch?v=xvqPEEpRBJ4) where I’ll show you how to work with API’s in Laravel. <br>
•	Author: Code With Dary <br>
•	Twitter: [@codewithdary](https://twitter.com/codewithdary) <br>
•	Instagram: [@codewithdary](https://www.instagram.com/codewithdary/) <br>


## Topics
•	Introduction to API’s <br>
•	HTTP Methods & Responses <br>
•	JSON Api Best Practices <br>
•	How to setup Laravel Passport <br>
•	How to create access tokens <br>
•	How to build resources <br>
•	CRUD REST API with Laravel <br>

## Usage <br>
Setup the repository <br>
```
git clone git@github.com:codewithdary/laravel-api-tutorial.git
cd bookstore
composer install
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

## What is an API?
I struggled a lot in the beginning when I started working with API’s and that was mainly because I couldn’t get a clear understand of what an API is. I always try to explain it with the following example

Imagine a restaurant where visitors (obviously) sit at a table and eat their food. The food is prepared by chefs in the kitchen. You can see the menu as an API, because visitors pick their food, and the kitchen will cook that. 

The API part makes it possible for a frontend developer to request a specific task or resource from the backend. When you want to order your food, you choose between different endpoints. When you order, you connect the backend of an application which will then send back some data.

## What is REST?
When working with API’s, you’ll be hearing the term ```REST``` a lot. Rest stands for: <br>
•	**R & E** – Representational <br>
•	**S** – State <br>
•	**T** – Transfer <br>

Rest is an architectural style that is used for communication between a server and a client. <br>
•	Rest uses HTTP, so Hypertext Transfer Protocol as a base communication style. <br>
•	REST data will be sent in either XML or JSON because they are both “Readable by humans”. <br>

## HTTP Methods
Available HTTP methods on a resource

| **Verb**        | **Path**           | **Action**  | **Route Name**        | **Description**   |
| ------------- |-------------| -----| ------------- |-------------|
| GET         | /movies | index | movies.index | Get all movies |
| GET         | /movies/create | create | movies.create | Get new created movies |
| POST         | /movies | store | movies.store | Create a new movie |
| GET         | /movies/{movie} | show | movies.show | Get data of specific movie |
| GET         | /movies/{movie}/edit | edit | movies.edit | Edit specific movie |
| PUT/PATCH         | /movies/{movie} | update | movies.update | Update a specific movies |
| DELETE         | /movies{delete} | destroy | movies.destroy | Delete a specific movie |


## Status codes

I won’t mention all status codes that we’ve got but if you are interested, I recommend you [this article from Laravel Daily](https://laraveldaily.com/laravel-api-errors-and-exceptions-how-to-return-responses/) where they show all available status codes.

## JSON Api Specification

Whether you’re making applications in Java, C#, python or you’re making web-based applications in Laravel, there’s a lot you need to think about regarding designing your software. A lot of developers tend to make small mistakes in their JSON file since it’s pretty easy to make small typos. Let’s go over the structure of a JSON file.

```ruby
{
       "id" : 1,
       "type": "books",
        "attributes": {
               "name": "Deathly Hallows",
               "publication_year": 2007
         },
        ”relationships": {
        }  
}

```

Every JSON document starts with an opening curly brace and ends with a closing curly brace.
Inside the fcurly braces, you need to have a comma to show separation

**Members** <br>
On the left side of the colon (:), we have something which is called a member name (```id```, ```type```, ```name```). There are some rules that you need to follow when defining members: <br>
•	Needs to have at least one character <br>
•	You can only use characters <br>
•	Member must start and end with global characters <br>
•	Keep your member names lowercase <br>
•	Use your own convention for snake cases, kebab case or camel case (But be consistent) <br>

**Content** <br>
The content of the value of the member. In our case, it will be ```1```, ```books```, ```Deathly Hallows```.

**Attributes** <br>
You have the option to pass in attributes inside a JSON specification. You can see attributes in here as the same attributes you use on your Laravel models: <br>
•	Attribute members should be an object <br>
•	Cannot be a relationship <br>
•	Attributes can be empty, or even better removed if you don’t use them <br>
•	In our example, ```name``` and ```publication_year``` as members inside our attributes <br>
•	There’s no need to add an id in your attribute because they are reserved on the root of the resource object (so outside of our attributes) <br>

**Relationships** <br>
Sometimes you might need a relationship inside your API because data in an application can be related to one another. 

## Setting up Laravel Passport

Laravel Passport is an Oauth 2.0 server implementation for API authentication using Laravel. IF you have ever worked with Google account, Spotify or Dropbox, you might have seen authentication. That’s being done through Oauth. Laravel Passport is used to secure your API through different ways of authentication.

Laravel Passport needs to be pulled in through Composer. You need to perform the following command from the root of your project directory:
```
composer require laravel/passport
```

Laravel Passport interacts with the database because the clients and access tokens need to be stored somewhere. You might think that nothing needs to be migrated since there are no new migrations, but the migrations of Laravel Passport are included in the composer package itself.
```
php artisan migrate
```

In order to run Laravel Passport, we need ot make sure that we run the following command to generate client information for us
```
php artisan passport:install
```

This will create a ```Client Id``` and ```Client Secret```
```ruby
Example: 
Client ID: 1
Client secret: 8yvyFECEuIVvZAJPmMGsG9EoZvgLfJKNMkaYKcFb
```

In order to find out which token and scope belongs to which user, we need to make sure that we connect with Laravel Passport in the correct way.

First of all, we need to tell the ```/bookstore/app/Models/User.php``` model that we want to pull in the hasApiTokens class:
``` ruby
use Laravel\Passport\HasApiTokens;
```

Also, make sure that you use the hasapiTokens as a trait inside your class:
``` ruby
use HasFactory, Notifiable, hasApiTokens;
```
The next step is to tell our ```/bookstore/app/Providers/AuthServiceProvider.php``` to revoke the access tokens, clients and personal access tokens. Let’s pull in ``Passport``` in the use statement
``` ruby
use Laravel\Passport\Passport;
```

In order to revoke the right items, we need to call the Passport routes inside the boot method of the ```/bookstore/app/Providers/AuthServiceProvider.php```

```ruby
public function boot()
{
    $this->registerPolicies();
    Passport::routes();
}
```

The last step is to setup Laravel Passport as a driver for our API Authentication. This can be done inside the ```/bookstore/config/Auth.php``` file, where you need to replace ```’driver’ => ‘token’ to:
```ruby
'api' => [
    'driver' => 'passport',
    'provider' => 'users',
    'hash' => false,
],
```
## Access Tokens

Instead of using the ```/bookstore/routes/web.php``` file, we’re going to use the ```/bookstore/routes/api.php``` file to define a new route for our API.

```ruby
Route::middleware('auth:api')->get('/user', function (Request $request) {
    return $request->user();
});
```
In postman, perform a ```GET``` request to ```http://127.0.0.1:8000/api/user. 

Whoops, we’ve got an error message. Before your endpoint works successfully, you got to make sure that you pass in the right data inside the ```headers``` tab of Postman. We got to make sure that our KEY is equal to ```Accept``` and the VALUE is ```application/json```

If you perform your request now, you’ll see that the status code is ```401```. If the output was the actual user, we would’ve got a huge problem. At the moment, the middleware is telling us that the authentication works properly, but the request has been performed by an unauthorized person. 

We need to make sure that we generate a unique access token that will allows us to perform requests.

There are lots of different ways on how you could fix this, and I want to use the two keys we generated in the previous section.

The first step is to perform a ```POST``` request to http://127.0.0.1:8000/oauth/token.

Click on the ```body``` tab inside Postman to pass in information inside the body of your request, and add the following:
```
grant_type:password
client_id: 1
client_secret: 8yvyFECEuIVvZAJPmMGsG9EoZvgLfJKNMkaYKcFb
username: [USERNAME FROM USERS TABLE]
password: [PASSWORD FROM USER]
scope:
```

If we save it and perform our request, you will see that the return value is 200 and we got a access token and refresh token.

If we perform the request one more time, you’ll see that the values are different this time.

Copy the access token, navigate to the ```Headers``` tab and create a new KEY VALUE pair.
The KEY will be ```Authorization``` and the value will be ```Bearer [CODE]```
If we perform a ```GET``` request to http://127.0.0.1:8000/api/v1/user, you’ll see all users available in the database.


## Building Resources

In order to create Authors for a bookstore, we need to make sure that we’ve got a Author model, database migration, factory and controller. Let’s perform the following command to create it:

```
php artisan make:model -a -r Author
```

Open the ```/bookstore/database/migrations/create_authors_table``` migration and replace the ```up()`` method with the following:
```ruby
public function up()
{
    Schema::create('authors', function (Blueprint $table) {
        $table->id();
        $table->string('name');
        $table->timestamps();
    });
}
```

We need to make sure that we set column ```name``` to fillable since we’re going to perform a mass assignment.
```ruby
protected $fillable ['name'];
```

Let’s migrate our database to create the ```authors``` table:
```
php artisan migrate
```

The next step is to add dummy data inside our database to create fake users! Let’s open the ``` /bookstore/database/factories/AuthorFactory.php``` and replace the ```definition()``` method with:
```ruby
public function definition()
{
    return [
        'name' => $this->faker->name
    ];
}
```

In order to use our factory, we got to make sure that we’ve got our database seeder setup
```
Php artisan make:seeder AuthorsTableSeeder
```

Open the ```/database/seeders/AuthorsTableSeeder.php``` file and replace the ```run()``` method with the following:
```ruby
public function run()
{
    $this->call(AuthorsTableSeeder::class);
}
```

Don’t forget to replace the model inside the  ```/database/seeder/DatabaseSeeder.php``` file:
```ruby 
public function run()
{
     \App\Models\Author::factory(10)->create();
}
```

Right now, we can migrate and seed our database:
```
php artisan migrate –seed
```

Let’s redefine our route ```/routes/api.php``` to the following:
```ruby
Route::middleware('auth:api')->prefix('v1')->group(function() {
    Route::get('/user', function(Request $request){
        return $request->user();
    });

    Route::apiResource('/authors', AuthorsController::class);
    Route::apiResource('/books', BooksController::class);
});
```

Everything after this part is straight forward and will be done inside the ```AuthorsResource```, ```Authorscontroller``` and ```BookSController``` files.

# Credits due where credits due…
Thanks to [Laravel](https://laravel.com/) for giving me the opportunity to make this tutorial on [Laravel API]( https://laravel.com/docs/5.8/api-authentication)


