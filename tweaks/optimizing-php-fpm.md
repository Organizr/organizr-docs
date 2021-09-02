# Optimizing PHP-FPM

## Summary

Get faster load times of tabs in Organizr.

## Windows

1. Open Command Prompt as an Administrator
2. Type the following command `set php` and you should see 2 variables
   1. `PHP_FCGI_CHILDREN=3`
   2. `PHP_FCGI_MAX_REQUESTS=128`
3. Run the following command to increase the value of `PHP_FCGI_CHILDREN`
   1. `SETX /m PHP_FCGI_CHILDREN 1000`
4. Close Command Prompt.
5. Open Command Prompt as Administrator and run `set php` and check that `PHP_FCGI_CHILDREN` value changed from `3` to `1000`.

Adapted from: [https://technicalramblings.com/blog/optimizing-php-fpm-to-get-faster-load-times-of-tabs-in-organizr/](https://technicalramblings.com/blog/optimizing-php-fpm-to-get-faster-load-times-of-tabs-in-organizr/)

