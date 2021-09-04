# Forgot my password

## Introduction

If you find yourself in the situation where you've forgotten the password to your Organizr account \(ONLY if you have auth set to Org Database ONLY and NOT using Plex or Emby backend\), and did not yet setup the PHPMailer Plugin to be able to reset your password, you can use the following instructions to configure the PHPMailer Plugin and regain the ability to reset your forgotten password.

{% hint style="info" %}
Starting with Organizr Version 2.1.165 you can now use the API to enable PHP Mailer with Organizr's SMTP account
{% endhint %}

## Version 2.1.165 and newer

With Organizr Version 2.1.165 and newer, there was a new API Endpoint added.  With this endpoint, you may now use Organizr's SMTP servers to reset your password if you have not setup PHPMailer yet.

In order to enable PHP Mailer you will need to know your Organizr API Key.  This is inside your `/api/config/config.php` file under the variable:

```text
'organizrAPI' => 'qefeh7de0poey7c87w0a',
```

Once you have the API Key you can navigate to this Organizr API Endpoint:

{% api-method method="get" host="http://organizr-instance" path="/api/v2/help/smtp?apikey=12345678901234567890" %}
{% api-method-summary %}
Organizr SMTP Helper
{% endapi-method-summary %}

{% api-method-description %}
Sets the smtp server account and credentials to Organizr's own smtp server account
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="apikey" type="string" required=true %}
Organizr's API Key
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
    "response": {
        "result": "success",
        "message": "SMTP activated with Organizr SMTP account",
        "data": true
    }
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
    "response": {
        "result": "error",
        "message": "Not Authorized",
        "data": null
    }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

Now you can go to Organizr and use the Forgot Password link.... That is it!

{% hint style="warning" %}
For versions below 2.1.165, If you do not have your own mail server to use, You can use Organizr's server... [Click Me!](https://api.organizr.app/zoho_smtp.php)
{% endhint %}

## Version below 2.1.165

Open up the Organizr config file, `/config/www/Dashboard/api/config/config.php`, in a text editor and setup the PHPMailer settings like so, with your own SMTP Server information:

```text
'PHPMAILER-enabled' => true,
'PHPMAILER-logo' => 'https://raw.githubusercontent.com/causefx/Organizr/v2-develop/plugins/images/organizr/logo-wide.png',
'PHPMAILER-smtpHost' => 'smtp.domain.com',
'PHPMAILER-smtpHostAuth' => true,
'PHPMAILER-smtpHostPassword' => 'NEEDSHASHEDPASSWORD',
'PHPMAILER-smtpHostPort' => '587',
'PHPMAILER-smtpHostSenderEmail' => '[email protected]',
'PHPMAILER-smtpHostSenderName' => 'Organizr',
'PHPMAILER-smtpHostType' => 'tls',
'PHPMAILER-smtpHostUsername' => '[email protected]',
'PHPMAILER-template' => 'light',
'PHPMAILER-verifyCert' => true
```

You will need to check with your e-mail provider for all of the correct settings for this to work with your e-mail account.

Make sure that, if the last line of the above code is the last line in the file, that there is NO comma at the end and that the new code is inside the PHP block, before the ending `);`.

For example, if you're appending the code to the end of your config file, it would end up looking like this:

```text
'PHPMAILER-enabled' => true,
'PHPMAILER-logo' => 'https://raw.githubusercontent.com/causefx/Organizr/v2-develop/plugins/images/organizr/logo-wide.png',
'PHPMAILER-smtpHost' => 'smtp.domain.com',
'PHPMAILER-smtpHostAuth' => true,
'PHPMAILER-smtpHostPassword' => 'NEEDSHASHEDPASSWORD',
'PHPMAILER-smtpHostPort' => '587',
'PHPMAILER-smtpHostSenderEmail' => '[email protected]',
'PHPMAILER-smtpHostSenderName' => 'Organizr',
'PHPMAILER-smtpHostType' => 'tls',
'PHPMAILER-smtpHostUsername' => '[email protected]',
'PHPMAILER-template' => 'light',
'PHPMAILER-verifyCert' => true

);
```

You will need the hashed value of the password, so, to get hashed value for `PHPMAILER-smtpHostPassword`

Before you do that, you will need your Organizr hash key.  This is inside your `/api/config/config.php` file under the variable:

```text
'organizrHash' => 'xxxxxxxxxx',
```

Once you have the `organizrHash` you can head over to: [Organizr's password hashing tool here](https://api.organizr.app/encrypt.php)

Put the hashed password into the `config.php` file and then you _**SHOULD**_ be able to recover/reset your Organizr account password.

