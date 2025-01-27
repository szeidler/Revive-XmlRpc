
# szeidler/Revive-XmlRpc

Remote procedure calls - RPC - is a basic API that has existed on Revive since it was OpenX Source.
This is a basic update to that system and extracted into a package which can be used in virtually any Php Project to access your Revive server or manage multiple Revive servers.
The response data is not exactly pretty for Ads Display, but hopefully I can come up with a version 3 that format the data into a more friendly format.

## Revive AdServer XML API

Extracted into a package and updated to use packages rather than pear

## Requires
- php-xml

## Uses 

- [phpoption/phpoption](https://github.com/schmittjoh/php-option)
- [phpxmlrpc/phpxmlrpc](https://github.com/gggeek/phpxmlrpc)
- [illuminate/support](https://github.com/illuminate/support)
- [vlucas/phpdotenv](https://github.com/vlucas/phpdotenv)

## Setup

### Composer

```json
    "require": {
        "szeidler/revive-xmlrpc": ^2"
    },
```

## Using the API

[phpdoc documentation](./api.md)

### API version 2 XML

these examples were tested in Laravel 5.6 Commands

#### Configuration

Loads definitions in this order
Each step will override/replace the previous step

1. loads /Assets/Config/revive-xmlrpc.php (defaults)
2. laravel style configs (if function config exists)
3. constructor array settings (first param can be an array)
4. constructor individual settings

##### Laravel config/env 
     
```bash
php artisan vendor:publish --provider=artistan/revive-xmlrpc
```

you can edit the `config/revive-xmlrpc.php` file add these env variables to or .env files

```dotenv
RVRPC_HOST=ads.me.com
RVRPC_BASEPATH=/api/v2/xmlrpc/
RVRPC_USERNAME=admin
RVRPC_PASSWORD=~test~
RVRPC_PORT=0
RVRPC_SSL=1
RVRPC_TIMEOUT=15
```


```php
use Artistan\ReviveXmlRpc\OpenAdsV2ApiXmlRpc;
$rpc = new OpenAdsV2ApiXmlRpc();
$list = $rpc->getAgencyList();
```

##### Custom config initialization

uses `./Assets/Config/revive-xmlrpc.php` for config if you do not provide it to the class.

```php
use Artistan\ReviveXmlRpc\OpenAdsV2ApiXmlRpc;

$config = [
	'host'=>'ads.me.com', 
	'basepath'=>'/www/api/v2/xmlrpc/',
	'username'=>'admin', 
	'password'=>'~test~', 
	'port'=>0, 
	'ssl'=>true, 
	'timeout'=>15
]


$rpc = new OpenAdsV2ApiXmlRpc($config);
$list = $rpc->getAgencyList();
```


##### Full initialization

```php
use Artistan\ReviveXmlRpc\OpenAdsV2ApiXmlRpc;
$rpc = new OpenAdsV2ApiXmlRpc('ads.me.com', '/www/api/v2/xmlrpc/', 'admin', '~test~', 0, true, 15);
$list = $rpc->getAgencyList();
```

### API version 1 XML

```php
use Artistan\ReviveXmlRpc\OpenAdsV1ApiXmlRpc;
$rpc = new OpenAdsV1ApiXmlRpc('ads.me.com', '/www/api/v1/xmlrpc/', 'admin', '~test~', 0, true, 15);
$list = $rpc->getAgencyList();
```

#### Ad Display Retrieval XML

```php
        $rpc = new OpenAdsDisplayXmlRpc('ads.me.com', '/www/delivery/axmlrpc.php', 443, true, 15);
        $rpc->setRemoteInfo('remote_addr', 'chuck-dev');
        $list = $rpc->view(
        	/* string    zone */ 'zone:1', 
        	/* int campaignid */ 0, 
        	/* string  target */ '', 
        	/* string  source */ '', 
        	/* 0|1   withText */ 0, 
        	/* array  contect */ [], 
        	/* strubg charset */ ''
		);
        var_dump(json_decode(json_encode($list),true));
```

## [Documentation](https://github.com/victorjonsson/PHP-Markdown-Documentation-Generator) updates

[PHP-Markdown-Documentation-Generator](https://github.com/victorjonsson/PHP-Markdown-Documentation-Generator)

```bash
./vendor/bin/phpdoc-md generate --ignore=test,examples src > api.md
```
