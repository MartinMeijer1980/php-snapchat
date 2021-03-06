Snapchat for PHP
================
[![Build Status](https://travis-ci.org/JorgenPhi/php-snapchat.png)](https://travis-ci.org/JorgenPhi/php-snapchat)

This library is built to communicate with the Snapchat API. It is nearly
feature complete but still lacks some functionality available in the latest
versions of the official apps (namely Stories).

It's similar to the [excellent Snaphax library](http://github.com/tlack/snaphax)
built by Thomas Lackner <[@tlack](http://twitter.com/tlack)>, but the approach
is different enough that I figured it deserved its own repo.


Usage
-----

Include src/snapchat.php via require_once or Composer or whatever, then:

```php
<?php

// Log in:
$snapchat = new Snapchat('username', 'password');

// Get your feed:
$snaps = $snapchat->getSnaps();

// Get your friends' stories:
$snaps = $snapchat->getFriendStories();

// Download a specific snap:
$data = $snapchat->getMedia('122FAST2FURIOUS334r');
file_put_contents('/home/dan/snap.jpg', $data);

// Download a specific story:
$data = $snapchat->getStory('[story_media_id]', '[story_key]', '[story_iv]');

// Download a specific story's thumbnail:
$data = $snapchat->getStoryThumb('[story_media_id]', '[story_key]', '[thumbnail_iv]');

// Mark the snap as viewed:
$snapchat->markSnapViewed('122FAST2FURIOUS334r');

// Mark the story as viewed:
$snapchat->markStoryViewed('[story_id]');

// Screenshot!
$snapchat->markSnapShot('122FAST2FURIOUS334r');

// Upload a snap and send it to me for 8 seconds:
$id = $snapchat->upload(
	Snapchat::MEDIA_IMAGE,
	file_get_contents('/home/dan/whatever.jpg')
);
$snapchat->send($id, array('stelljes'), 8);

// Upload a video story:
$id = $snapchat->upload(
	Snapchat::MEDIA_VIDEO,
	file_get_contents('/home/dan/whatever.mov')
);
$snapchat->setStory($id, Snapchat::MEDIA_VIDEO);

// Destroy the evidence:
$snapchat->clearFeed();

// Find friends by phone number:
$friends = $snapchat->findFriends(array('18006492568', '7183876962'));

// Get a list of your friends:
$friends = $snapchat->getFriends();

// Add some people as friends:
$snapchat->addFriends(array('bill', 'bob'));

// Add someone you forgot:
$snapchat->addFriend('bart');

// Get a list of the people you've added:
$added = $snapchat->getAddedFriends();

// Find out who Bill and Bob snap the most:
$bests = $snapchat->getBests(array('bill', 'bob'));

// Set Bart's display name:
$snapchat->setDisplayName('bart', 'Barty');

// Block Bart:
$snapchat->block('bart');

// Unblock Bart:
$snapchat->unblock('bart');

// Delete Bart entirely:
$snapchat->deleteFriend('bart');

// You only want your friends to be able to snap you:
$snapchat->updatePrivacy(Snapchat::PRIVACY_FRIENDS);

// You want to change your email:
$snapchat->updateEmail('dan@example.com');

// Log out:
$snapchat->logout();

?>
```

##Snaptcha

This includes the unfinished "Snaptcha" implementation including two new methods,

getCaptcha()

and

sendCaptcha()

Example:

```php
<?php

include 'src/snapchat.php';

// dummy variables.

$email = "spevanegiel@gmail.com";
$password = "snapchat1001";
$birthday = "1963-05-04";
$username = "spevenspatula01";

$s = new Snapchat();

// register normally..

$s->register($email,$password,$birthday);

// register your desired username..

$s->register_username($email, $username);

// verify yourself...

$captcha_id = $s->getCaptcha($username, true);

// ask the user for the captcha, 
// (should be replaced with respected ghost images)...
//
// returns false if unable to retrive captcha_id.

var_dump($captcha_id);

echo "captcha?";

$solution = fgets(STDIN); // solution is 8 characters in binary. eg. 001010011

// send off captcha.

$s->sendCaptcha($solution, $captcha_id, $username);

?>
```


Documentation
------------

There is none, but I tried to mark up the code well enough to make up for it.
Error handling is pretty weak, so watch for that.


License
------------

MIT
