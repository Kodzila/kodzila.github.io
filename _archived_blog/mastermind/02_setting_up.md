# Setting up

## Installing Symfony CLI
Let's go to [Symfony page](https://symfony.com/download) and follow the instructions.

## Initializing project
Thanks to Symfony CLI we can initialize new Symfony project:
```
symfony new mastermind
```

The initial structure now should be created. Let's see what is inside:
```
.
├── bin
├── config
├── public
├── src
├── var
├── vendor
├── .env
├── .gitignore
├── composer.json
├── composer.lock
└── symfony.lock
```

## Working with composer

### Requirements policy
Before:
```json
 "require": {
        "php": "^7.2.5",
        "ext-ctype": "*",
        "ext-iconv": "*",
        "symfony/console": "5.0.*",
        "symfony/dotenv": "5.0.*",
        "symfony/flex": "^1.3.1",
        "symfony/framework-bundle": "5.0.*",
        "symfony/yaml": "5.0.*"
    },
```

After:
```json
 "require": {
        "php": "^7.2.5",
        "ext-ctype": "*",
        "ext-iconv": "*",
        "symfony/console": "5.0.*",
        "symfony/dotenv": "5.0.*",
        "symfony/flex": "1.3.*",
        "symfony/framework-bundle": "5.0.*",
        "symfony/yaml": "5.0.*"
    },
```

### Scripts
A good way of handling common task is to integrate them via composer scripts. By default Symfony initialize composer
script section with such a configuration:
```json
    "scripts": {
        "auto-scripts": {
            "cache:clear": "symfony-cmd",
            "assets:install %PUBLIC_DIR%": "symfony-cmd"
        },
        "post-install-cmd": [
            "@auto-scripts"
        ],
        "post-update-cmd": [
            "@auto-scripts"
        ]
    },
```

You can add new tasks. Let's add a new task `lint` which will be executing all tools used to ensure best quality of our
code. You will read more about linting in next articles.
```json
"scripts": {
        "auto-scripts": {
            "cache:clear": "symfony-cmd",
            "assets:install %PUBLIC_DIR%": "symfony-cmd"
        },
        "post-install-cmd": [
            "@auto-scripts"
        ],
        "post-update-cmd": [
            "@auto-scripts"
        ],
        "lint": [
            "echo 'linting...'"
        ]
    },
```

Now you can execute your task by running:
```bash
composer lint
```

