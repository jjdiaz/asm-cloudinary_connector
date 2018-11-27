# Cloudinary extension for Akeneo PIM

[![Build Status](https://travis-ci.org/textmaster/akeneo-extension.svg?branch=master)](https://travis-ci.org/textmaster/akeneo-extension)

Not available on the Akeneo marketplace: 

## Description

Cloudinary connector allows you to connect to Cloudinary DAM by your credentials
## Requirements

| Akeneo Textmaster extension | Akeneo PIM Community Edition |
|:---------------------------:|:----------------------------:|
| v1.0.*                      | v2.3.* + API template        |
                       |

You also need a Cloudinary account to have some API credentials and access to the Textmaster's customer interface.

### Create a Cloudinary account

Creating your account on Cloudinary is totally free. You can access the register form by clicking on the "Login" button or by following [this link](https://cloudinary.com/).


## How it works

![mass edit screen](doc/img/mass-edit-01.png)

Explanation of functionalities


## Installation

First step is to require the sources:
```
composer require jjdiaz/asm-cloudinary_connector
```

Register your bundle in the `AppKernel::registerProjectBundles`:

```
new \Asm\Bundle\CloudinaryBundle\AsmCloudinaryConnectorBundle(),
```

Then we need to add a new mass edit batch job:

```
bin/console akeneo:batch:create-job 'Textmaster Connector' 'textmaster_start_projects' "mass_edit" 'textmaster_start_projects'
```

Add the new routes used by the extension to the global router. Add the following lines at the end of `app/config/routing.yml`:

```
textmaster:
    resource: "@PimTextmasterBundle/Resources/config/routing.yml"
```

Update the database schema and regenerate your cache and assets:

```
rm bin/cache/* -rf
bin/console doctrine:schema:update --force
rm -rf web/bundles/* web/css/* web/js/* ; bin/console pim:install:assets
```

Finally, you must set a `cron` to retrieve the translated contents from Textmaster:
```
0 * * * * /home/akeno/pim/bin/console pim:textmaster:retrieve-translations >> /tmp/textmaster.log
```

This command checks for translated content once every hour. We do not recommend to check more often than every hour to not overload the Textmaster servers.

### Parameters

You can configure your TextMaster plugin in the dedicated screen: `System >> Configuration >> TextMaster`

![configuration screen](doc/img/configuration-01.png)

In this screen you will be able to set:

- you API credentials : `API key` and `API secret`
- the attributes you want to translate

## Screenshots

![Select products](doc/img/01-select-products.png)

![Select Textmaster action](doc/img/02-select-action.png)

![Configure the project](doc/img/03-configure-project.png)

![Execution details](doc/img/04-execution-details.png)

## Video demo

A live demonstration for the 1.2 version of this extension is available on this short video:
https://www.youtube.com/watch?v=9WkyQFwoWWo
