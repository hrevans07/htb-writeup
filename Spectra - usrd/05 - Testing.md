## Enumeration
* Doing a gobuster on the /testing directory shows that it is a wordpress site but just the source code
* We know the version of wordpress on /main is 5.4.2
* Download all the files on /testing and the src of wordpress 5.4.2 and do a diff -> shows that almost all files are the same except for a few
```bash
┌──(hunter㉿kali)-[~/htb/spectra/wp-src]                               
└─$ diff -q testing/ wordpress/                                          
Only in testing/: index.html                                  
Only in wordpress/: index.php                                                 
Only in wordpress/: wp-activate.php                                         
Common subdirectories: testing/wp-admin and wordpress/wp-admin  
Only in wordpress/: wp-blog-header.php                      
Only in wordpress/: wp-comments-post.php                        
Only in testing/: wp-config.php.save                        
Only in wordpress/: wp-config-sample.php                                       
Common subdirectories: testing/wp-content and wordpress/wp-content                                                                                            
Only in wordpress/: wp-cron.php                                                                                                                               
Common subdirectories: testing/wp-includes and wordpress/wp-includes
Only in wordpress/: wp-links-opml.php                                        
Only in wordpress/: wp-load.php                         
Only in wordpress/: wp-login.php                                          
Only in wordpress/: wp-mail.php                                          
Files testing/wp-settings.php and wordpress/wp-settings.php differ             
Only in wordpress/: wp-signup.php                           
Only in wordpress/: wp-trackback.php                                     
Files testing/xmlrpc.php and wordpress/xmlrpc.php differ 
```
* The important files here are the ones that only appear in testing, since we think it's a mirror of the /main site
* Specifically, the file *wp-config.php.save*
* Look in this file to get some db creds
```sql
// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define( 'DB_NAME', 'dev' );

/** MySQL database username */
define( 'DB_USER', 'devtest' );

/** MySQL database password */
define( 'DB_PASSWORD', 'devteam01' );

/** MySQL hostname */
define( 'DB_HOST', 'localhost' );

/** Database Charset to use in creating database tables. */
define( 'DB_CHARSET', 'utf8' );

/** The Database Collate type. Don't change this if in doubt. */
define( 'DB_COLLATE', '' );
```
* mysql:devtest:devteam01