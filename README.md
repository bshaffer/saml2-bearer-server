## Saml2-bearer authorization grant Server

This is a demo project that implements an Oauth2 server that handles saml2-bearer authorization grants. It mainly uses bshaffer/oauth2-server-php, onelogin/php-saml and codeguy/Slim

### Oauth2-server saml2-bearer authorization grant

We use onelogin/saml to help us with the hard xml validations. I found that is not easy to find a tool in php to make standar-compliant saml2 tokens. Take a look at the comments at the [saml2 config file ] (https://github.com/aacotroneo/saml2-bearer-server/blob/master/inst/saml_settings.php) 

### Slim and Oauth2-server

The only problem you may face to integrate both projects is that they both have their own Request and Response. It's never a good thing to have 2 different objects hanging around which represent the same thing, especially the response, which is handled a bit differenty in each library. In slim you just have to echo the response (it's stored with ob_start), while on Oauth2 server you have to 'send()' it explicitely.

To overcome this issue I used a simple adapter pattern, but through inheritance instead of composition so that it is compilant with both interfaces. For example take a look at the [Response Adapter](https://github.com/aacotroneo/saml2-bearer-server/blob/master/src/Oauth2/Http/ResponseAdapter.php) which is the most interesting. Note that Oauth2 server adds params to the response, and slim willl automatically call finalize() when it's the right time. If therer are unsent params, we write a Json, so you won't have to call send() on responses anymore. That's it! 

Thank you for reading, and let me know if you any questions, suggestions!


