Telegram Login
==============
Telegram Login for Websites

Installation
------------

The preferred way to install this extension is through [composer](http://getcomposer.org/download/).

Either run

```
php composer.phar require --prefer-dist mdisepehr/yii2-telegram-login "*"
```

or add

```
"mdisepehr/yii2-telegram-login": "*"
```

to the require section of your `composer.json` file.

Bot Config
-----

for first you must add a bot in [@Botfather](https://t.me/botfather) and add domain with /setdomain command

or read [this link](https://core.telegram.org/widgets/login)

Login Button
-----

for use login button :

```php
<?php 
$token='XXXXX:XXXXXXXXXXXXXXXXXXXXX';
$login=new \mdisepehr\telegram\Login('sampleBot',$token); 
echo $login->loginButton('check-authorization','large');
?>
```

Check Authorization
-----

for check authorization in your action of controller:


```php
<?php 
$token='XXXXX:XXXXXXXXXXXXXXXXXXXXX';
$login=new \mdisepehr\telegram\Login('sampleBot',$token); 
$auth_data=$login->checkTelegramAuthorization(Yii::$app->request->get());

$id=$auth_data['id'];
$first_name=$auth_data['first_name'];
$last_name=$auth_data['last_name'];
$photo_url=$auth_data['photo_url'];
?>
```


for login in yii2:

```php
<?php
if(($user=Users::findOne(['username'=>'tel_'.$auth_data['id']]))===null){
     $user=new Users();
     $user->username='tel_'.$auth_data['id'];
     $user->name=$auth_data['first_name'].(isset($auth_data['last_name'])?" {$auth_data['last_name']}":"");
     $user->authKey=Yii::$app->security->generateRandomString(12);
     $user->chat_id=$auth_data['id'];
     $user->save(false);
}

Yii::$app->user->login(User::findByUsername($user->username),3600*24*30);

return $this->goHome();
?>
```