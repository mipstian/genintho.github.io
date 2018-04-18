---
layout: post
title:  "Class Laravel\Spark\Providers\SparkServiceProvider not found"
date: 2018-04-17 10:00:00
---

While updating some composer dependencies on a Laravel 5.6 using Laravel Spark, I encounter the following error:

```
@php artisan package:discover

In ProviderRepository.php line 208:
                                                                                                                                Class 'Laravel\Spark\Providers\SparkServiceProvider' not found  

Script @php artisan package:discover handling the post-autoload-> > dump event returned with error code 1
```

Spark was installed using the Spark Installer (as opposed to composer way).

A quick way to fix the issue is to remove the `spark` symlink in `vendor/laravel`.

> rm vendor/laravel/spark

Then re-running `composer update` worked without problem.



