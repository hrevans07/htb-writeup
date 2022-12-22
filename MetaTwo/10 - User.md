Land in WP and see this users permissions are pretty limited. There's almost none of the admin functionality that would be present if this user was an admin.
![[Pasted image 20221107154225.png]]
Do some more research on this version of WP and find an arbitrary file read via XXE through the Media manager. I used this repo for exploiting it because I didn't feel like writing my own tool: https://github.com/motikan2010/CVE-2021-29447
Use this file read vulnerability to read ```wp-config.php``` which is located in the web root. This file yields some more creds, specifically for the DB and FTP server.
```php
<?php
/** The name of the database for WordPress */
define( 'DB_NAME', 'blog' );

/** MySQL database username */
define( 'DB_USER', 'blog' );

/** MySQL database password */
define( 'DB_PASSWORD', '635Aq@TdqrCwXFUZ' );

/** MySQL hostname */
define( 'DB_HOST', 'localhost' );

/** Database Charset to use in creating database tables. */
define( 'DB_CHARSET', 'utf8mb4' );

/** The Database Collate type. Don't change this if in doubt. */
define( 'DB_COLLATE', '' );

define( 'FS_METHOD', 'ftpext' );
define( 'FTP_USER', 'metapress.htb' );
define( 'FTP_PASS', '9NYS_ii@FyL_p5M2NvJ' );
define( 'FTP_HOST', 'ftp.metapress.htb' );
define( 'FTP_BASE', 'blog/' );
define( 'FTP_SSL', false );

[TRUNCATED]
?>
```
Use the FTP creds to login to the FTP server and find 2 folders: blog and mailer. Check out the mailer directory and see one file: ```send_email.php```
Download that file and read it to get some more creds:
```php
[Truncated]
$mail->Host = "mail.metapress.htb";
$mail->SMTPAuth = true;
$mail->Username = "jnelson@metapress.htb";
$mail->Password = "Cb4_JmWM8zUZWMu@Ys";
$mail->SMTPSecure = "tls";
$mail->Port = 587;
[Truncated]
```
Use these creds to get an SSH session as user ```jnelson``` 