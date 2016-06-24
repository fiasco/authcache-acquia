# Authcache Acquia
Authcache module integration on Acquia Cloud Enterprise. This is a drop in replacement module for the authcache varnish module specifically for Acquia Cloud.

This module relies on the [digest module](https://github.com/varnish/libvmod-digest) in Varnish to provide HMAC authentication on incoming requests from authenticated users. 

## Installation
Prior to attempting to use this module, please be sure to read the Acquia documentation on [Custom Varnish configuration for Acquia Cloud Enterprise sites](https://docs.acquia.com/cloud/performance/custom-varnish). 

This module is designed to be used on Acquia Cloud Enterprise. It requires modifications to a dedicated load balancer pair in order for the module to work. With out this change, **this module will not work**.

Add this module into your codebase along with the [Authcache module](https://www.drupal.org/project/authcache). You'll need to submit an Acquia Support Ticket against your subscription to request a VCL change. Provide them the `acquia_default-real-vcl.patch` patch file as the change that is required.

### Note
`acquia_default-real-vcl.patch` **requires** a modification. You must place your own Drupal private key inside the patch so that Varnish knows this key also. You can obtain this key by running the following command against the site you which to provide authenticated caching for:

```
$ drush vget drupal_private_key
```
Place the key accordingly on the line that looks like this:

```patch
+    set req.http.drupal_private_key = "<INSERT YOUR DRUPAL PRIVATE KEY HERE>";
```

Authcache module comes with a module called Authcache Varnish. You will not need this module. It should not be enabled.

## How it works
![Diagram](https://raw.githubusercontent.com/fiasco/authcache-acquia/master/Acquia%20Authcache%20-%20HTTP%20Workflow.png)
