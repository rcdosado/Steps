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

12. Now edit this migration file like this:

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