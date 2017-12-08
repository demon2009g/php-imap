# PHP IMAP

[![Author](http://img.shields.io/badge/author-@barbushin-blue.svg?style=flat-square)](https://www.linkedin.com/in/barbushin)
[![GitHub release](https://img.shields.io/github/release/barbushin/php-imap.svg?maxAge=86400&style=flat-square)](https://packagist.org/packages/php-imap/php-imap)
[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE)
[![Packagist](https://img.shields.io/packagist/dt/php-imap/php-imap.svg?maxAge=86400&style=flat-square)](https://packagist.org/packages/php-imap/php-imap)

### Features

* Connect to mailbox by POP3/IMAP/NNTP, using [PHP IMAP extension](http://php.net/manual/book.imap.php)
* Get emails with attachments and inline images
* Get emails filtered or sorted by custom criteria
* Mark emails as seen/unseen
* Delete emails
* Manage mailbox folders
 
### Requirements

* IMAP extension must be present; so make sure this line is active in your php.ini: `extension=php_imap.dll | extension=php_imap.so`

### Installation by Composer

	$ composer require php-imap/php-imap
	
### Integration with frameworks

* Symfony - https://github.com/secit-pl/imap-bundle

### Usage example

```php
// 4. argument is the directory into which attachments are to be saved:
$mailbox = new PhpImap\Mailbox('{imap.gmail.com:993/imap/ssl}INBOX', 'some@gmail.com', '*********', __DIR__);

// Read all messaged into an array:
$mailsIds = $mailbox->searchMailbox('ALL');
if(!$mailsIds) {
	die('Mailbox is empty');
}

// Get the first message and save its attachment(s) to disk:
$mail = $mailbox->getMail($mailsIds[0]);

print_r($mail);
echo "\n\nAttachments:\n";
print_r($mail->getAttachments());
```

### Example "Get messages in all Folders for the current day"

```php
// 4. argument is the directory into which attachments are to be saved:
$mailbox = new PhpImap\Mailbox('{imap.yandex.ru:993/ssl/novalidate-cert/readonly}', "some@yandex.ru", "*********", null);

foreach($mailbox->getListingFolders() as $folder){

	echo "<h1>{$folder}</h1><br/>";

	$mailbox->switchMailbox($folder);

	// Read all messaged into an array:
	$mailsIds = $mailbox->searchMailbox('SINCE "'.date("d F Y").'"');	
	rsort($mailsIds);

	foreach($mailsIds as $k=>$id){
		$mail = $mailbox->getMail($id);
		var_dump($mail->headers->toaddress);
		var_dump($mail->headers->fromaddress);
		var_dump($mail->headers->reply_toaddress);
		var_dump($mail->headers->senderaddress);
		var_dump($mail->fromName);
		var_dump($mail->fromAddress);
		var_dump($mail->subject);
		echo($mail->textPlain);
		echo($mail->textHtml);
		echo "<hr/>";
	}
		
}
```

### Recommended

* Google Chrome extension [PHP Console](https://chrome.google.com/webstore/detail/php-console/nfhmhhlpfleoednkpnnnkolmclajemef)
* Google Chrome extension [JavaScript Errors Notifier](https://chrome.google.com/webstore/detail/javascript-errors-notifie/jafmfknfnkoekkdocjiaipcnmkklaajd)
