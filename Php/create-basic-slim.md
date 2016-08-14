#Slim Project

Creating Basic Slim Project
--------------------------------

1. Install Wamp64 (preferably in root directory)
2. Install Composer (make sure it is in the PATH)
3. Create a folder from your WWW root, go there using terminal
3. Install Slim (you may use command below, assumes php.exe is in the PATH)

  ```bash
  composer require slim/slim "^3.0"
  ```

4. Create folders named public, app, app->api, and files app->api->post.php, app->db.php
5. Create .htaccess file in the public folder use this contents

	```bash
	ReWriteEngine On
	ReWriteCond %{REQUEST_FILENAME} !-f
	ReWriteCond %{REQUEST_FILENAME} !-d
	ReWriteRule ^ index.php [QSA, L]

	```
6. Copy the test code in slimframework.com, to index.php
7. open terminal and go to public directory, execute `php -S localhost:8080` to execute local host server
8. To Test if Slim is working go to the address bar of the browser type localhost:8080/hello/roy and enter

