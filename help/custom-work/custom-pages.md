---
description: >-
  You are able to create your own PHP and/or HTML/JS Pages to be loaded in the
  tab's module.
---

# Custom Pages

## Setup

To achieve this, you need to create a page file named anything you want inside the `pages` folder in the `data` folder.  The `data` folder is located in the root folder of Organizr.

Once you have created a blank page, you may paste this skeleton code to get you started:

```php
<?php
/*
 * Make sure to edit "name_here" with your page name - i.e. custom_code_presentation
 * You will edit on both "$GLOBALS['organizrPages'][] = 'name_here';" and "function get_page_name_here($Organizr)"
 */
$GLOBALS['organizrPages'][] = 'name_here';
function get_page_name_here($Organizr)
{
	if (!$Organizr) {
		$Organizr = new Organizr();
	}
	/*
	 * Take this out if you dont care if DB has been created
	 */
	if ((!$Organizr->hasDB())) {
		return false;
	}
	/*
	 * Take this out if you dont want to be for admin only
	 */
	if (!$Organizr->qualifyRequest(1, true)) {
		return false;
	}
	return '
			<script>
				// Custom JS here
			</script>
			<div class="">
				<div class="col-lg-12">
					<div class="panel bg-org panel-info">
						<div class="panel-heading">
							<span lang="en">Template</span>
						</div>
						<div class="panel-wrapper collapse in" aria-expanded="true">
							<div class="panel-body bg-org">Ayyy.... yooo...!
							</div>
						</div>
					</div>
				</div>
			</div>
		';
}
```



* You need to change the first instance of `name_here` on the `GLOBALS` variable. &#x20;
* Also make the change on the function name.
  * They need to be the exact same words.
* If you need DB Access, keep the if statement that has the `hasDB` method.
* If you need to check access by group, leave or change the first parameter in the `qualifyRequest` method to the cooresponding group ID
* Make the changes to the return with your code.



## Accessing Tab

Once you have the code setup, you may access it via API or via Tab Editor.



### API

{% swagger method="get" path="page" baseUrl="https://organizr-url/api/v2/" summary="This Endpoint will list all pages available" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-response status="200: OK" description="" %}
```
{
    "response": {
        "result": "success",
        "message": null,
        "data": [
            "dependencies",
            "error",
            "homepage",
            "lockscreen",
            "login",
            "settings_customize_appearance",
            "settings_customize_settings",
            "settings_image_manager",
            "settings_plugins_disabled",
            "settings_plugins_enabled",
            "settings_plugins_settings",
            "settings_plugins",
            "settings_settings_backup",
            "settings_settings_logs",
            "settings_settings_main",
            "settings_settings_sso",
            "settings_tab_editor_categories",
            "settings_tab_editor_homepage_order",
            "settings_tab_editor_homepage",
            "settings_tab_editor_tabs",
            "settings_template",
            "settings_user_manage_groups",
            "settings_user_manage_users",
            "settings",
            "tabs",
            "settings_wizard",
            "name_here"
            
            
        ]
    }
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="get" path="name_here" baseUrl="https://organizr-url/api/v2/page/" summary="Displays the code of your custom page" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-response status="200: OK" description="" %}
```
{
    "response": {
        "result": "success",
        "message": null,
        "data": "\r\n\t\t\t<script>\r\n\t\t\t\t// Custom JS here\r\n\t\t\t</script>\r\n\t\t\t<div class=\u0022\u0022>\r\n\t\t\t\t<div class=\u0022col-lg-12\u0022>\r\n\t\t\t\t\t<div class=\u0022panel bg-org panel-info\u0022>\r\n\t\t\t\t\t\t<div class=\u0022panel-heading\u0022>\r\n\t\t\t\t\t\t\t<span lang=\u0022en\u0022>Template</span>\r\n\t\t\t\t\t\t</div>\r\n\t\t\t\t\t\t<div class=\u0022panel-wrapper collapse in\u0022 aria-expanded=\u0022true\u0022>\r\n\t\t\t\t\t\t\t<div class=\u0022panel-body bg-org\u0022>Ayyy.... yooo...!\r\n\t\t\t\t\t\t\t</div>\r\n\t\t\t\t\t\t</div>\r\n\t\t\t\t\t</div>\r\n\t\t\t\t</div>\r\n\t\t\t</div>\r\n\t\t"
    }
}


```
{% endswagger-response %}
{% endswagger %}

### Tab Editor

Create a new tab just like this

<figure><img src="../../.gitbook/assets/image (88).png" alt=""><figcaption></figcaption></figure>

The Tab URL will be as follows: `api/v2/page/name_here` where `name_here` is the name of your new custom page.

After you have saved the tab, change the Tab Type to `Organizr` and enable the tab.

<figure><img src="../../.gitbook/assets/image (89).png" alt=""><figcaption></figcaption></figure>

### Enjoy

That's it!  Enjoy your new Custom Tab...
