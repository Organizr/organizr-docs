---
description: This page will go through how to create a Plugin for Organizr
---

# Plugin Development

{% hint style="warning" %}
This only pertains to Organizr instances on version 2.1.1140 or higher
{% endhint %}

## Folder Structure

Each plugin consists of the following files:

| File Name               | Description                                                                                                                                   |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| plugin.php              | The file is the PHP Class file that will have all your plugins functions and everything                                                       |
| api.php                 | This file will contain all the API routes for your plugin                                                                                     |
| config.php              | This file will contain all of your plugins default config values                                                                              |
| page.php \[OPTIONAL]    | This file can hold some html that Organizr can display if you set a Tab in Organizr as _<mark style="color:purple;">**Organizr**</mark>_ type |
| main.js                 | This Javascript file will be included when Organizr loads if your plugin is enabled                                                           |
| settings.js \[OPTIONAL] | This Javascript file will be included when Organizr loads the settings page if your plugin is enabled                                         |
| logo.png                | The logo file for your plugin                                                                                                                 |

## File Development

### plugin.php

```php
<?php
// PLUGIN INFORMATION
$GLOBALS['plugins']['Test'] = [ // Plugin Name
	'name' => 'Test', // Plugin Name
	'author' => 'CauseFX', // Who wrote the plugin
	'category' => 'Testing', // One to Two Word Description
	'link' => '', // Link to plugin info
	'license' => 'personal', // License Type use , for multiple
	'idPrefix' => 'TEST', // html element id prefix (All Uppercase)
	'configPrefix' => 'TEST', // config file prefix for array items without the hypen (All Uppercase)
	'version' => '1.0.1', // SemVer of plugin
	'image' => 'api/plugins/test/logo.png', // 1:1 non transparent image for plugin
	'settings' => true, // does plugin need a settings modal?
	'bind' => true, // use default bind to make settings page - true or false
	'api' => 'api/v2/plugins/test/settings', // api route for settings page (All Lowercase)
	'homepage' => false // Is plugin for use on homepage? true or false
];

class TestPlugin extends Organizr
{
	public function _pluginGetSettings()
	{
		return [
			'Sample Settings' => [
				$this->settingsOption('password-alt', 'TEST-pass-alt',['label' => 'Test Plugin Pass Alt']),
				$this->settingsOption('password', 'TEST-password',['label' => 'Test Plugin Password']),
				$this->settingsOption('text', 'TEST-text',['label' => 'Test Plugin Text'])
			],
			'FYI' => [
				$this->settingsOption('html', 'HTML Note', ['html' => '<span lang="en">This is just a note</span>']),
			]
		];
	}
}
```

The first thing you will notice is that there is a Global variable set with a `plugins` array.  For this example, our Plugin name is `Test`.

Also notice on the array we again specify the name of the plugin.  Here is a breakdown of the rest of the array keys:

| Key          | Value Type | Description                                                                                                            |
| ------------ | ---------- | ---------------------------------------------------------------------------------------------------------------------- |
| author       | String     | Your name or pseudonym                                                                                                 |
| category     | String     | Overall type of Plugin                                                                                                 |
| link         | String     | Link to additional info for plugin, can be blank                                                                       |
| license      | String     | <p>Can be either personal or business.  Can also be both just use csv.  </p><p>I.E. <code>personal,business</code></p> |
| idPrefix     | String     | This is used for HTML elements, please use all Uppercase and one word only                                             |
| configPrefix | String     | This is used for config items so they don't clash with other items.  Please use all Uppercase and no spaces            |
| version      | String     | SemVer of your plugin                                                                                                  |
| image        | String     | Path to your logo                                                                                                      |
| settings     | Boolean    | Does your plugin need a settings modal                                                                                 |
| bind         | Boolean    | Use the default bind to make settings modal                                                                            |
| api          | String     | API Route to settings values                                                                                           |
| homepage     | Boolean    | NOT IN USE YET                                                                                                         |

Now that we have finished setting up your Plugin's Global variable array, we can now continue to creating your Plugin's PHP Class.

If you plan on using some of Organizr built in functions and User information, you will need to extend the Organizr Class.  Otherwise, you can just create a new class and that is it.

If you want a settings modal, you will need to create a function to supply which config items you will want to set up for your settings modal.  Let's start with creating a function to pass an array with our config items. &#x20;

First, you need to name your function something, let's call this one `_pluginGetSettings`, we can also set this function as public if you choose to share those variables with other classes etc...

Next, we will return an array with our Setting's groups.  In this example our first Group is called `Sample Settings`.  Inside this Settings group, we will have 3 config items that we want to setup.  Each item is setup via the `settingsOption` function.  This function takes 3 parameters. &#x20;

{% hint style="warning" %}
public function OptionsFunction::settingsOption($type, $name = null, $extras = null) array|string\[]
{% endhint %}

The first option is the type of config item that we are setting up.  The second, is the name of the config item. Lastly, we can provide the sometimes-optional extra options needed to set the item up.

Let's take a look at each one.

| Config Item Name | Config Type  | Extra Options |
| ---------------- | ------------ | ------------- |
| TEST-pass-alt    | password-alt | label         |
| TEST-password    | password     | label         |
| TEST-text        | text         | label         |
| N/A              | html         | html          |

You can find out all the type of config item options under the `options-functions.php` file located under `Organizr Location/api/functions/` folder.

### config.php

```php
<?php
/*
 * Always include PLUGINNAME-enabled
 * Along with all your other settings from plugin.php
 */
return [
	'TEST-enabled' => false,
	'TEST-pass-alt' => '',
	'TEST-password' => '',
	'TEST-text' => ''
];
```
