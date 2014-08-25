# AWS Service Provider for Cilex

A simple Cilex service provider for including the [AWS SDK for PHP](https://github.com/aws/aws-sdk-php).

This is literally just a version of the [AWS Silex Provider](http://github.com/aws/aws-sdk-php-silex) modified for Cilex.

## Usage

Register the AWS Service Provider in your Cilex application and provide your AWS SDK for PHP configuration to the app
in the `aws.config` key. `$app['aws.config']` should contain an array of configuration options or the path to a
configuration file. This value is passed directly into `Aws\Common\Aws::factory()`.

```php
<?php

require __DIR__ . '/vendor/autoload.php';

use Aws\Cilex\AwsServiceProvider;
use Cilex\Application;

$app = new Application();

$app->register(new AwsServiceProvider(), array(
    'aws.config' => array(
        'key'    => 'your-aws-access-key-id',
        'secret' => 'your-aws-secret-access-key',
        'region' => 'us-east-1',
    )
));
// Note: You can also specify a path to a config file
// (e.g., 'aws.config' => '/path/to/aws/config/file.php')

$app->match('/', function () use ($app) {
    // Get the Amazon S3 client
    $s3 = $app['aws']->get('s3');

    // Create a list of the buckets in your account
    $output = "<ul>\n";
    foreach ($s3->getListBucketsIterator() as $bucket) {
        $output .= "<li>{$bucket['Name']}</li>\n";
    }
    $output .= "</ul>\n";

    return $output;
});

$app->run();
```

## Links

* [AWS Silex Provider](http://github.com/aws/aws-sdk-php-silex)
* [AWS SDK for PHP on Github](http://github.com/aws/aws-sdk-php)
* [AWS SDK for PHP website](http://aws.amazon.com/sdkforphp/)
* [AWS on Packagist](https://packagist.org/packages/aws)
* [License](http://aws.amazon.com/apache2.0/)
