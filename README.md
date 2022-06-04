<center><h1>Simple PHP Notes</h1></center>

<h2>File Upload</h2>

> php $_FILES[ ]  properties 
```php

	$_FILES['file']['name'] // file name
	$_FILES['file']['type'] // file type
	$_FILES['file']['tmp_name'] // file temp location
	$_FILES['file']['size'] // file size
	$_FILES['file']['error'] // is there any error?

```
<br>

> HTML Form
```html

	<form  action="result.php"  method="post"  enctype="multipart/form-data">

	<input  type="file"  name="file">
	<input  type="submit"  name="send"  value="Send">

	</form>

```
<br>

> Handle File in PHP file
```php

	if(isset($_FILES['file'])):

		$error = $_FILES['file']['error'];

		if($error != 0):

			echo 'error while upload'; // error check

			else:

			$size = $_FILES['file']['size']; // file size check

			if($size > (1024*1024)):

				echo 'file cannot be higher than 5MB';

			else:

				$name = $_FILES['file']['name'];
				/*
				$extension = explode(".",$name);
				$extension = $extension[count($extension) - 1]; // file type check
				*/
				$type = $_FILES['file']['type'];
				$acceptableTypeArray = array("image/png"); // you can change acceptable file type!

				if(!in_array($type, $acceptableTypeArray)):

					echo 'invalid file type';

				else:

					$fileLocation = $_FILES['file']['tmp_name'];
					move_uploaded_file($fileLocation, "your/path");
					echo 'file uploaded succesfully!';

				endif;
			endif;
		endif;
	endif;

```

<hr>

### Sessions 

> if we want to use session on our webpages, we have to call session_start() function where start of the file. 

```php

	session_start();
	$_SESSION['session_variable'] =  'variable-data'; // create session variable
	// this process is similar to define a variable
	// but defined variable stores in browser 
	$data = $_SESSION['session_variable'];  // get session variable 
	
```
> You can define an array in SESSION variable. 

<br>

```php

	unset($_SESSION['session_variable']);  // deletes "session_variable" variable!

```

```php

	// Timed Session
	
	$_SESSION["timed"] = time() + 5; // added 5 sec
	$_SESSION["var1"] = "xyz"; 
	
	if($_SESSION["timed"] < time()):
		unset($_SESSION["var1"]);
	endif; 
	// var1 depends timed session-variable

```



#
### Cookies 

> We can set when its die. 

```php
	// SET
	setcookie('variable1','data1'); // when browser close, this variable will die.
	setcookie('variable2','data2', time() + (60*60*24)); // We can set when its die.
	
	// CALL
	$varX = $_COOKIE['variable1'];

	// KILL 
	setcookie('variable1','', time() - 10); // thats enough for kill the cookie.


```

#
### Final, Self, Parent

`Final`--> If any class has final keyword at start, we cant extend from it. This situations means is "this class is finished or I dont want to anybody change it".


```php

	final class Animal{
	
	}
	
	class Cat extends Animal{
		// It doesnt work!
	}
	
```
`Self` With self:: we can access functions which defines in the same class and static defined variables.

```php

		class Myth{
		public static $var1 = 'kadir';
		public function fun1(){
			echo 'xyz';
			echo self::$var1;
			self::fun1(); // loop :)
		}
		
		}
		
```

`parent` We can get the features of the classic that we extended.


```php
	
	class  Myth{  
	public  static  $var1  =  'kadir'; 
	
	public  function  fun1(){
	echo  'xyz';
	echo self::$var1; 
	self::fun1();  // loop :) 
	
	}}
	
```

### Namespace Usage

We  uses `namespace` keyword for use the classes have the same name in the same file. 

```php
	
	//define first.php
	namespace first;
	class firstClass{
	
		static function hello(){
			echo "first namespace";
		}
	}
	

```
```php
	
	// define second.php 
	namespace second;
	class firstClass{
	
	static function second(){
		echo "second namespace";
	}
	}

```

```php
	
	require_once 'first.php';
	require_once 'second.php';
	$first_object = new namespace\first\firstClass();
	$second_object = new namespace\second\firstClass();

```


### MYSql Connection 

We will use PDO class for connect to database.

```php
	
	try{

		$db  =  new  PDO("mysql:host=dbServer;dbname=databaseName;charset=utf8", "username", "password");
		$db->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);

	}

	catch(PDOException  $e){
		die($e->getMessage());

	}
	
```

### Select 
```php
	
	$query  =  $db->prepare("select * from users where id=1")->fetch(PDO::FETCH_ASSOC);
	$query-> execute();
	echo  $query['id'];
	
	// for multiple rows 
	
	$q = "SELECT * FROM users";
	$stmt = $conn->prepare( $q );
	$stmt->execute();
	echo '<ul>';
	while( $row = $stmt->fetch() ) {
		echo '<li>' . $row['first_name'] . '</li>';
	}

```

### Insert

```php

	$name  =  "kadir";
	$surname  =  "yaren";
	
	$query  =  $db->prepare("INSERT  INTO tablename(name,surname) VALUES(?,?)");
	$query->execute(array($name,$surname));

```

### Update
```php

	$name2 = "arda";
	$surname2 = "jack";
	$query = $db->prepare("UPDATE tablename SET name=?, surname=? WHERE id=1");
	$query->execute(array($name2,$surname2));

```

### Delete 

```php
	
	$id = 1;
	$query = $db->prepare("DELETE FROM tablename WHERE id=?");
	$query->execute(array($id));

```

### Join

```php

	$query = $db->prepare("SELECT * FROM table1 JOIN table2 ON table1.name = table2.ad");
	$query->execute();

```



### PHPMailer

```php
	
	use PHPMailer\PHPMailer\PHPMailer;
	use PHPMailer\PHPMailer\Exception;
	
	require  '../lib/PHPMailer-master/src/PHPMailer.php';
	require  '../lib/PHPMailer-master/src/Exception.php';
	require  '../lib/PHPMailer-master/src/SMTP.php';
	$mail  =  new  PHPMailer(true);

	try {

		//Server settings
		$mail->SMTPDebug  =  0;
		$mail->Charset  =  'UTF-8'; //Enable verbose debug output
		$mail->isSMTP(); //Send using SMTP
		$mail->Host  =  'smtp.gmail.com'; //Set the SMTP server to send through
		$mail->SMTPAuth  =  true; //Enable SMTP authentication
		$mail->Username  =  'info.notesup@gmail.com'; //SMTP username
		$mail->Password  =  'LEvandovski35!'; //SMTP password
		$mail->SMTPSecure  =  'tls'; //Enable TLS encryption; `PHPMailer::ENCRYPTION_SMTPS` encouraged
		$mail->Port  =  587; //TCP port to connect to, use 465 for `PHPMailer::ENCRYPTION_SMTPS` above
		  
		//Recipients

		$mail->setFrom('info.notesup@gmail.com', 'NotesUp!');
		$mail->addAddress($mailLast); //Add a recipient
		$mail->addReplyTo('info.notesup@gmail.com', 'User Replies');
		/*$mail->addCC('cc@example.com');*/
		$mail->addBCC($mailLast);

		//Attachments
		/*$mail->addAttachment('/var/tmp/file.tar.gz'); //Add attachments
		$mail->addAttachment('/tmp/image.jpg', 'new.jpg'); //Optional name*/
		//Content

		$mail->isHTML(true); //Set email format to HTML
		$mail->Subject  =  ' NotesUp Account Verification'; // localhost/pages/activate.php?process='.$confirmKeyLast.'
		$mail->Body  =  'Please go to this link to activate your account! : <a style="text-decoration:none !important; color:blue" href="noteapp.duckdns.org'.$activation.'">ACTIVATE!</a> noteapp.duckdns.org'.$activation.'';
		/*$mail->AltBody = 'This is the body in plain text for non-HTML mail clients';*/
		$mail->send();

	  

	} catch (Exception  $e) {

		echo  "Message could not be sent. Mailer Error: {$mail->ErrorInfo}";

	}
	


```
## PHP JSON API BE CAREFUL TO HEADER
```php

 	header("Access-Control-Allow-Headers: *");
    	header("Access-Control-Allow-Origin: *");

```


 



 










