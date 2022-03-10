# WDN Symfony Framework

***Notice: This framework is currently a prototype and a WIP.***

Uses the [Symfony Bundle System](https://symfony.com/doc/current/bundles.html)
to provide WDN PHP twig templates to be used for application layout. The twig
templates use `include` to pull in current WDN template includes and `block` to
assign markup/content to WDN template editable areas.

This framework bundle is dependent on the [DCF Symfony Framework](https://github.com/digitalcampusframework/dcf-symfony-framework) 
to provide common DCF base controller and partial templates.  Other DCF may be 
available later.

## Version Syncing ##
The WDN layout templates should be maintained in the [WDN Repo](https://github.com/unl/wdntemplates) and not
in this repo.  When updates are made to WDN you the maintainers of this repo should 
use the `sync-version.php` script to update the layout templates for a specific WDN
major version (i.e. 5.3).

Example: `php sync-version.php 5.3` to sync `5.3`.

Any updates can then be committed to this branch.

## Implementation ##

### composer.json ###

Add to `require` section:
```
"digitalcampusframework/dcf-symfony-framework": "dev-main",
"unl/wdn-symfony-framework": "dev-main"
```
run `composer install`

### Config ###
Add the following to `config/bundles.php` return array:
```
DCF\Bundle\FrameworkBundle\DCFFrameworkBundle::class => ['all' => true],
WDN\Bundle\FrameworkBundle\WDNFrameworkBundle::class => ['all' => true],
```
This tells the application which bundles to use.

### WDN Files ###
Add symlink to WDN in symphony project's `templates` and `public` directories so
application can read in WDN includes, css, and javascript.  You can achieve this
in other ways if desired.

### Create Base Application Template ###
Add `wdnbase.html.twig` (or whatever name) file to project's `templates` directory. 

Use this file to set application's default template block values. These can be
overwritten in each template file if necessary.

Sample file:
```
{# templates/wdnbase.html.twig #}
{% extends '@WDNFramework/5.3/php.app_local.dwt.twig' %}

{% block head %}
<link rel="stylesheet" href="{{ asset('css/test.css') }}">
{% endblock %}

{% block highlighted %}
{{ include('@DCFFramework/partials/flash-notices.html.twig') }}
{% endblock %}

{% block jsbody %}
<script>
    WDN.setPluginParam("idm", "logout", "/logout/");
    WDN.initializePlugin("notice");
</script>
{% endblock %}

{# Default Block Values #}
{% block doctitle %}<title>Test App</title>{% endblock %}
{% block titlegraphic %}<a class="dcf-txt-h5" href="/">Test App</a>{% endblock %}
{% block affiliation %}<a href="http://ucomm.unl.edu">University Communication</a>{% endblock %}
{% block breadcrumbs %}
    <ol>
        <li><a href="https://www.unl.edu/">Nebraska</a></li>
        <li><a href="/">Chat Administration</a></li>
    </ol>
{% endblock %}
{% block navlinks %}
    <ul>
        <li><a href="/">Home</a></li>
        <li><a href="/lucky">Lucky</a></li>
        <li><a href="/help">Help</a></li>
    </ul>
{% endblock %}
{% block appcontrols %}
<ul>
    <li><a href="/">Home</a></li>
    <li><a href="/lucky">Lucky</a></li>
    <li><a href="/help">Help</a></li>
</ul>
{% endblock %}
{% block contactinfo %}
<div id="dcf-footer-group-1">
    <h3 class="dcf-txt-md dcf-bold dcf-uppercase dcf-lh-3 unl-ls-2 unl-cream" id="dcf-footer-group-1-heading">About the Directory</h3>
    <ul class="dcf-list-bare dcf-mb-4" aria-labelledby="dcf-footer-group-1-heading">
        <li><a href="<?php echo UNL_Peoplefinder::getURL() . 'help/' ?>"><svg class="dcf-mr-1 dcf-h-4 dcf-w-4 dcf-fill-current" aria-hidden="true" focusable="false" height="16" width="16" viewBox="0 0 24 24"><path d="M11.5 1.004C5.158 1.004 0 6.162 0 12.504s5.158 11.5 11.5 11.5c6.341 0 11.5-5.158 11.5-11.5s-5.159-11.5-11.5-11.5zm-.5 5a1.001 1.001 0 11-.002 2.002A1.001 1.001 0 0111 6.004zm3.5 14h-6a.5.5 0 010-1h2.499v-8H9.5a.5.5 0 010-1h2a.5.5 0 01.499.5v8.5H14.5a.5.5 0 010 1z"></path></svg>Directory Help</a></li>
        <li><a href="https://wdn.unl.edu/documentation/unl-directory"><svg class="dcf-mr-1 dcf-h-4 dcf-w-4 dcf-fill-current" aria-hidden="true" focusable="false" height="16" width="16" viewBox="0 0 24 24"><path d="M15 0H9v20.707l3-3 3 3zM8 0H5.5a.5.5 0 00-.5.5v23a.5.5 0 00.853.354L8 21.707V0zm8 0h2.5a.5.5 0 01.5.5v23a.501.501 0 01-.854.354L16 21.707V0z"></path></svg>Developer Documentation</a></li>
    </ul>
    <p>Information obtained from this directory may not be used to provide addresses for mailings to students, faculty or staff. Any solicitation of business, information, contributions or other response from individuals listed in this publication by mail, telephone or other means is forbidden.</p>
    <p>This application is a product of the <a href="https://dxg.unl.edu/">Digital Experience Group at Nebraska</a>. DXG is a partnership of <a href="https://ucomm.unl.edu/">University Communication</a> and <a href="https://its.unl.edu/">Information Technology Services</a>.</p>
    <p>This website is protected against malicious content robots and abusive web browsers. If you experience an issue or receive an error/blocked message while browsing, please immediately report the problem to the <a href="https://its.unl.edu/helpcenter/">ITS Help Center</a>.</p>
</div>
<div id="dcf-footer-group-2">
    <h3 class="dcf-txt-md dcf-bold dcf-uppercase dcf-lh-3 unl-ls-2 unl-cream" id="dcf-footer-group-2-heading">Related Links</h3>
    <ul class="dcf-list-bare dcf-mb-0" aria-labelledby="dcf-footer-group-2-heading">
        <li><a href="https://wdn.unl.edu/">Web Developer Network</a></li>
        <li><a href="https://dxg.unl.edu/">Digital Experience Group</a></li>
        <li><a href="https://ucomm.unl.edu/">University Communication</a></li>
        <li><a href="https://its.unl.edu/">Information Technology Services</a></li>
    </ul>
</div>
{% endblock %}
```
Highlights from file:
* `{% extends '@WDNFramework/5.3/php.app_local.dwt.twig' %}` tells which layout to use.
* `breacrumbs` and `navlinks` are only for `php.local.dwt.twig`
* `appcontrols` is only for `hp.app_local.dwt.twig`
* These blocks will vary over time per WDN version.

### Controllers ###
Extend WDN Framework in application's controllers

Example
```
<?php
namespace App\Controller;

use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;
use WDN\Bundle\FrameworkBundle\Controller\BaseController;

class MyController extends BaseController
{
```

### Templates ###
Use application's base template in templates

Example:
```
{% extends 'wdnbase.html.twig' %}

{% block title %}Help Page{% endblock %}
{% block jsbody %}
    {{ parent() }}
    <script>
      console.log('hello');
    </script>
{% endblock %}

{% block content %}
<div class="dcf-mt-6 dcf-mb-6">
    <p>Hello World!!</p>
</div>
{% endblock %}
```
