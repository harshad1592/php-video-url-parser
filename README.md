# PHP Video URL Parser
PHP Video URL Parser is a parser that detects a given video url and returns an object containing information like the video's embed code, title, description, thumbnail and other information that the service's API may give.

## Installation

Install the latest version with

```bash
$ composer require copadia/php-video-url-parser
```

## Requirements

* PHP 5.3
* cURL (Or at least file_get_contents() enabled if you want to use it with Vimeo, otherwise it's not required)

## Basic Usage

```php
<?php
use RicardoFiorani\Matcher\VideoServiceMatcher;

require __DIR__ . '/vendor/autoload.php';

$vsm = new VideoServiceMatcher();

//Detects which service the url belongs to and returns the service's implementation
//of RicardoFiorani\Adapter\VideoAdapterInterface
$video = $vsm->parse('https://www.youtube.com/watch?v=PkOcm_XaWrw');

//Checks if service provides embeddable videos (most services does)
if ($video->isEmbeddable()) {
    //Will echo the embed html element with the size 200x200
    echo $video->getEmbedCode(200, 200);

    //Returns the embed html element with the size 1920x1080 and autoplay enabled
    echo $video->getEmbedCode(1920, 1080, true);

    //Returns the embed html element with the size 1920x1080, autoplay enabled and force the URL schema to be https.
    echo $video->getEmbedCode(1920, 1080, true, true);
}

//If you don't want to check if service provides embeddable videos you can try/catch
try {
    echo $video->getEmbedUrl();
} catch (\RicardoFiorani\Exception\NotEmbeddableException $e) {
    die(sprintf("The URL %s service does not provide embeddable videos.", $video->getRawUrl()));
}

//Gets URL of the smallest thumbnail size available
echo $video->getSmallThumbnail();

//Gets URL of the largest thumbnail size available
//Note some services (such as Youtube) does not provide the largest thumbnail for some low quality videos (like the one used in this example)
echo $video->getLargestThumbnail();
```

## Currently Suported Services
* Youtube
* Vimeo
* Dailymotion
* Facebook Videos

## Currently Supported PHP Versions
* PHP 5.3
* PHP 5.4
* PHP 5.5
* PHP 5.6
* PHP 7.0
* PHP 7.1
