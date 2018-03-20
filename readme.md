
# PwnedPasswords
A library to query Troy Hunt's Pwned Passwords service to see whether or not a password has been included in a public breach.

# Requirements

 - PHP >= 5.0.0
 - ext-curl

# Installation
Installing PwnedPasswords is made easy via Composer. Just require the package using the command below, and you are ready to go.

    composer require ron-maxweb/pwned-passwords
    
# Usage
To use the library, you can do something along the lines of the following.
```php
require_once('vendor/autoload.php');

$pp = new PwnedPasswords\PwnedPasswords;

$password = '123456789';

$insecure = $pp->isInsecure($password);

var_dump($insecure);
```
The `isInsecure` method will return true if the password has been found in the PwnedPasswords API, and false if not.

If you want to build your own thresholds (Ex. display a warning if the password has been found more than once and an error if more than 5x) you can call the `isInsecure` method like below.
```php
$pp = new PwnedPasswords\PwnedPasswords;

$password = '123456789';

$insecure = $pp->isInsecure($password);

var_dump($insecure);

if($insecure) {
  $count = $pp->getCount($password);
  echo 'Oh no — pwned!' . "\n";
  echo sprintf('This password has been seen %d time%s before.',$count,($count > 1 ? 's' : ''));
} else {
  echo 'All good !';
}
```

By default `PwnedPasswords` uses `curl_*` to fetch result, and `file_get_contents` if the curl request fails, you can specify  the method to use like this : 

```php
use PwnedPasswords;
/**
* uses curl, and file_get_contents if curl request failed
* @throws \RuntimeException if both methods failed
*/
$pp = new PwnedPasswords; 

/**
* uses curl
* @throws \RuntimeException if both curl request failed
*/
$pp = new PwnedPasswords(PwnedPasswords::CURL); 

/**
* uses file_get_contents
* @throws \RuntimeException if failed to open stream
*/
$pp = new PwnedPasswords(PwnedPasswords::FILE); 
```
you can also supply the curl options.
example : 

```php
use PwnedPasswords;

$options = [
  CURLOPT_CERTINFO => true,
  CURLOPT_FRESH_CONNECT => true,
  CURLOPT_SSL_VERIFYPEER => true,
  CURLOPT_SSL_VERIFYSTATUS => true
];

$pp = new PwnedPasswords(null,$options);
// or 
$pp = new PwnedPasswords(PwnedPasswords::CURL,$options);
```

# Issues
Please feel free to use the Github issue tracker to post any issues you have with this library.
