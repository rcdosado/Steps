#Lumen Project

Creating Basic Lumen REST Project
--------------------------------

1. Install Wamp64 (preferably in root directory)
2. Install Composer (make sure it is in the PATH)
3. Create a folder from your WWW root, go there using terminal
4. Install Lumen (you may use command below, assumes php.exe/composer is in the PATH)

  ```bash
  composer create-project laravel/lumen lumen_rest_project
  ```
   Composer will create a folder "lumen_rest_project" and install all files of lumen including dependency.
5. verify if correctly installed by issuing `php artisan` in the console, the command also lets you see other commands
6. Now run Lumen on your local server and open localhost:8000 in your browser
  ```bash
  php artisan serve
  ```
7. Create Database in MySQL, named it like "lumen_rest" for the sake of the tutorial
8. Write account information for the following:

  ```
	DB_CONNECTION=mysql
	DB_HOST=localhost
	DB_DATABASE=lumen_rest
	DB_USERNAME=root
	DB_PASSWORD=s3cr3t  
  ```
9. find .env.example, and renamed to .env, replace those values with yours
10. Now open "lumen_rest_project/bootstrap/app.php" and uncomment the ff lines:

  ```
	Dotenv::load(__DIR__.'/../');  
	$app->withFacades();
	$app->withEloquent();
  ```

11. design your database using php artisan command like this
  ```
	php artisan make:migration create_books_table -create=books
  ```
  the entity is books, so change it depending on your model, the command above will create a migration file under "database/migration". if you open the file, you'll find "CreateBooksTable" class has been created, this class has two method one is "up" where we'll write our table schema, another is "down", where we'll drop our table which will call at the time of rollback.

12. Now edit this migration file like this (this is just an example, change it accordingly)


  ```php
	use Illuminate\Database\Schema\Blueprint;
	use Illuminate\Database\Migrations\Migration;
	 
	class CreateBooksTable extends Migration {
	 
		/**
		 * Run the migrations.
		 *
		 * @return void
		 */
		public function up()
		{
			Schema::create('books', function(Blueprint $table)
			{
				$table->increments('id');
				$table->string('title');
				$table->string('author');
				$table->string('isbn');
				$table->timestamps();
			});
		}
	 
		/**
		 * Reverse the migrations.
		 *
		 * @return void
		 */
		public function down()
		{
			Schema::drop('books');
		}
	 
	}

  ```
13. You have to tell lumen to update the structure of the database by issuing:

   ```bash
	php artisan migrate
   ```

14. Now create a Book model under "app/Book.php" and use book table instance.
   ```php

	<?php namespace App;
	  
	use Illuminate\Database\Eloquent\Model;
	  
	class Book extends Model
	{
		 
		 protected $fillable = ['title', 'author', 'isbn'];
		 
	}
	?>

   ```
15. Also create a BookController.php under "app/Http/Controllers".

   ```php
	<?php
	  
	namespace App\Http\Controllers;
	  
	use App\Book;
	use App\Http\Controllers\Controller;
	use Illuminate\Http\Request;
	  
	  
	class BookController extends Controller{
	  
	 .....
	 .....
	 
	}
	?>
   ```

16. open "app/Http/routes.php" , you'll find one routed already exists.


   ```php
	$app->get('/', function() use ($app) {
	..
   ```
    Write some routes and corresponding Controller method in order to create RESTful API.


	|Method	|Url                                    |Controler@method	        |
	|-------|---------------------------------------|-------------------------------|------------------
	|GET	|http://localhost:8000/api/v1/book      |BookController@index           |All Books
	|GET	|http://localhost:8000/api/v1/book{id}  |BookController@getbook         |Fetch Book By id
	|POST	|http://localhost:8000/api/v1/book      |BookController@createBook      |Create a book record
	|PUT	|http://localhost:8000/api/v1/book{id}	|BookController@updateBook      |Update Book By id
	|DELETE	|http://localhost:8000/api/v1/book{id}	|BookController@deleteBook      |Delete Book By id


   we have appended api/v1 (ie, version v1) in all routes. It's a good practice in order to create web services.

17. write this in your routes.php

   ```php
	$app->get('/', function() use ($app) {
		return "Lumen RESTful API By CoderExample (https://coderexample.com)";
	});
	$app->get('api/v1/book','App\Http\Controllers\BookController@index');
	$app->get('api/v1/book/{id}','App\Http\Controllers\BookController@getbook');
	$app->post('api/v1/book','App\Http\Controllers\BookController@createBook');
	$app->put('api/v1/book/{id}','App\Http\Controllers\BookController@updateBook');
	$app->delete('api/v1/book/{id}','App\Http\Controllers\BookController@deleteBook');
   ```