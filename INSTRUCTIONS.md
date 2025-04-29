# Using the Forked shiki-php Package

This guide explains how to use the forked version of shiki-php in your Laravel project, allowing you to specify a custom Node.js binary path.

## Step 1: Update Your composer.json

Add the repository and requirement to your project's `composer.json`:

```json
"require": {
    "openagentsinc/shiki-php": "dev-main"
},
"repositories": [
    {
        "type": "vcs",
        "url": "https://github.com/OpenAgentsInc/shiki-php"
    }
]
```

## Step 2: Update Your Dependencies

Run composer update to install the forked package:

```bash
composer update openagentsinc/shiki-php
```

## Step 3: Configure the Node Binary Path

You have multiple options for configuring the Node binary path:

### Option 1: Using Environment Variables

Add this to your `.env` file:

```
NODE_BINARY_PATH=/path/to/your/node
```

This is the simplest approach and works well for deployment environments where you know the Node binary location.

### Option 2: Setting the Path Programmatically

Add this code to a service provider (like `AppServiceProvider.php`):

```php
use Spatie\ShikiPhp\Shiki;

public function boot()
{
    Shiki::setCustomNodePath('/path/to/your/node');
}
```

This is useful when you need to dynamically determine the node path based on your environment.

### Option 3: Default Node Lookup (Fallback)

If neither of the above methods are used, the package will look for the node executable in:
- `/usr/local/bin`
- `/opt/homebrew/bin`
- `$HOME/n/bin` (for the Node version manager 'n')

## Laravel Markdown Integration

If you're using Laravel Markdown, this customized shiki-php will be used automatically if you've required it in your composer.json as specified above.

## Laravel Cloud and Other Environments

For environments where you can't modify the system or create symlinks:

1. Determine the location of node in your environment
2. Set the `NODE_BINARY_PATH` environment variable to that location
3. Make sure the node binary is accessible by the PHP process

For Laravel Cloud, you might need to:
- Add the NODE_BINARY_PATH environment variable in your project settings
- Or modify your deployment script to find and set the node path before your application runs