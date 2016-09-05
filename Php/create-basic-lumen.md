#Lumen Project



Creating Basic Lumen REST Project
---------------------------------

   `from https://coderexample.com/restful-api-in-lumen-a-laravel-micro-framework/`


1. Install Wamp64 or similar (preferably in root directory), this is optional, you can use `php -S localhost:9000` as webserver, but you must have MySQL installed and running
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
   you can make this compact by using group like the following:

   ```php
	$app->get('/', function() use ($app) {
		return "Lumen RESTful API By CoderExample (https://coderexample.com)";
	});
	 
	$app->group(['prefix' => 'api/v1','namespace' => 'App\Http\Controllers'], function($app)
	{
		$app->get('book','BookController@index');
	  
		$app->get('book/{id}','BookController@getbook');
		  
		$app->post('book','BookController@createBook');
		  
		$app->put('book/{id}','BookController@updateBook');
		  
		$app->delete('book/{id}','BookController@deleteBook');
	});

   ```

18. create a controller

   ```php

	<?php
	  
	namespace App\Http\Controllers;
	  
	use App\Book;
	use App\Http\Controllers\Controller;
	use Illuminate\Http\Request;
	  
	  
	class BookController extends Controller{
	  
	  
		public function index(){
	  
			$Books  = Book::all();
	  
			return response()->json($Books);
	  
		}
	  
		public function getBook($id){
	  
			$Book  = Book::find($id);
	  
			return response()->json($Book);
		}
	  
		public function createBook(Request $request){
	  
			$Book = Book::create($request->all());
	  
			return response()->json($Book);
	  
		}
	  
		public function deleteBook($id){
			$Book  = Book::find($id);
			$Book->delete();
	 
			return response()->json('deleted');
		}
	  
		public function updateBook(Request $request,$id){
			$Book  = Book::find($id);
			$Book->title = $request->input('title');
			$Book->author = $request->input('author');
			$Book->isbn = $request->input('isbn');
			$Book->save();
	  
			return response()->json($Book);
		}
	  
	}
   ```

19. Test it, this is just one of many ways to test your API, you can also use POSTMAN chrome extension

   ```bash

	curl -I http://localhost:8000/api/v1/book
	 
	curl -v -H "Accept:application/json" http://localhost:8000/api/v1/book/2
	 
	curl -i -X POST -H &quot;Content-Type:application/json&quot; http://localhost:8000/api/v1/book -d '{&quot;title&quot;:&quot;Test Title&quot;,&quot;author&quot;:&quot;test author&quot;,&quot;isbn&quot;:&quot;12345&quot;}'
	 
	curl -v -H "Content-Type:application/json" -X PUT http://localhost:8000/api/v1/book -d '{"title":"Test updated title","author":"test upadted author","isbn":"1234567"}'
	 
	curl -i -X DELETE http://localhost:8000/api/v1/book/2

   ```

20. you can install plugins by simply using composer at the terminal.


   The code and most instructions here are not mine, this is from a tutorial from https://coderexample.com/restful-api-in-lumen-a-laravel-micro-framework/ i just arranged it so its more direct. 