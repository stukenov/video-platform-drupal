# Video Platform Drupal

A video hosting platform built on Drupal 11 CMS. This project provides a content management system for video hosting with Drupal's powerful content modeling, user management, and extensibility features.

## Features

- **Drupal 11 Core**: Latest stable version of Drupal CMS
- **Content Management**: Flexible content types for video management
- **User Management**: Built-in user roles and permissions
- **Media Library**: Drupal's native media handling capabilities
- **Multilingual Support**: Content translation and localization
- **RESTful API**: Headless CMS capabilities
- **Extensible**: Easy to extend with custom modules and themes
- **Security**: Regular security updates and best practices

## Tech Stack

- **CMS**: Drupal 11.x
- **Language**: PHP 8.2+
- **Database**: MySQL 8.0+ / MariaDB 10.6+ / PostgreSQL 13+
- **Web Server**: Apache 2.4+ / Nginx 1.18+
- **Composer**: Dependency management

## Prerequisites

- PHP 8.2 or higher
- Composer 2.x
- MySQL 8.0+ or MariaDB 10.6+ or PostgreSQL 13+
- Apache 2.4+ or Nginx 1.18+
- Minimum 256MB RAM (512MB recommended)

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/stukenov/video-platform-drupal.git
cd video-platform-drupal
```

### 2. Install Dependencies

```bash
composer install
```

### 3. Configure Environment

```bash
cp .env.example .env
# Edit .env with your database credentials and settings
```

### 4. Configure Database

Edit `web/sites/default/settings.php` and add your database configuration:

```php
$databases['default']['default'] = [
  'database' => 'your_database_name',
  'username' => 'your_database_user',
  'password' => 'your_database_password',
  'host' => 'localhost',
  'port' => '3306',
  'driver' => 'mysql',
  'prefix' => '',
  'collation' => 'utf8mb4_general_ci',
];
```

### 5. Install Drupal

Visit your site in a web browser and follow the installation wizard, or use Drush:

```bash
vendor/bin/drush site:install standard \
  --db-url=mysql://username:password@localhost/dbname \
  --site-name="Video Platform" \
  --account-name=admin \
  --account-pass=admin
```

### 6. Set File Permissions

```bash
chmod 755 web/sites/default
chmod 644 web/sites/default/settings.php
mkdir -p web/sites/default/files
chmod 775 web/sites/default/files
```

## Project Structure

```
video-platform-drupal/
├── composer.json         # Composer dependencies
├── composer.lock        # Locked dependency versions
├── web/                 # Web root
│   ├── core/           # Drupal core files
│   ├── modules/        # Contributed and custom modules
│   │   ├── contrib/   # Contributed modules
│   │   └── custom/    # Your custom modules
│   ├── themes/        # Themes
│   │   ├── contrib/   # Contributed themes
│   │   └── custom/    # Your custom themes
│   ├── profiles/      # Installation profiles
│   ├── sites/         # Site-specific files
│   │   └── default/   # Default site
│   │       ├── files/ # Uploaded files
│   │       └── settings.php # Site configuration
│   └── .htaccess      # Apache configuration
├── vendor/            # Composer dependencies
└── recipes/          # Drupal recipes
```

## Configuration

### Database Setup

The project uses Drupal's standard database configuration. Update `web/sites/default/settings.php`:

```php
$databases['default']['default'] = [
  'database' => getenv('DB_NAME') ?: 'drupal_db',
  'username' => getenv('DB_USER') ?: 'drupal_user',
  'password' => getenv('DB_PASSWORD') ?: '',
  'host' => getenv('DB_HOST') ?: 'localhost',
  'port' => getenv('DB_PORT') ?: '3306',
  'driver' => getenv('DB_DRIVER') ?: 'mysql',
  'prefix' => getenv('DB_PREFIX') ?: '',
];
```

### Trusted Host Settings

For security, configure trusted host patterns in `settings.php`:

```php
$settings['trusted_host_patterns'] = [
  '^localhost$',
  '^127\.0\.0\.1$',
  '^example\.com$',
  '^www\.example\.com$',
];
```

### File Paths

Configure public and private file paths in `settings.php`:

```php
$settings['file_public_path'] = 'sites/default/files';
$settings['file_private_path'] = '../private';
$settings['file_temp_path'] = '/tmp';
```

## Development

### Using Drush

Drush is included as a Composer dependency. Common commands:

```bash
# Clear cache
vendor/bin/drush cache:rebuild

# Export configuration
vendor/bin/drush config:export

# Import configuration
vendor/bin/drush config:import

# Update database
vendor/bin/drush updatedb

# Install module
vendor/bin/drush pm:install module_name

# Create user
vendor/bin/drush user:create username --mail="user@example.com" --password="password"
```

### Module Development

Create custom modules in `web/modules/custom/`:

```bash
mkdir -p web/modules/custom/my_module
cd web/modules/custom/my_module
# Create .info.yml, .module files
```

### Theme Development

Create custom themes in `web/themes/custom/`:

```bash
mkdir -p web/themes/custom/my_theme
cd web/themes/custom/my_theme
# Create .info.yml, template files
```

## Adding Modules

Add contributed modules via Composer:

```bash
composer require drupal/module_name
vendor/bin/drush pm:install module_name
```

Recommended modules for video platform:
- `drupal/media`: Media management
- `drupal/video_embed_field`: Embed videos from YouTube, Vimeo
- `drupal/file_mdm`: File metadata management
- `drupal/views`: Create custom content listings

## Production Deployment

### Performance Optimization

1. Enable caching in `settings.php`:
```php
$config['system.performance']['cache']['page']['max_age'] = 3600;
$config['system.performance']['css']['preprocess'] = TRUE;
$config['system.performance']['js']['preprocess'] = TRUE;
```

2. Clear cache before deployment:
```bash
vendor/bin/drush cache:rebuild
```

3. Set proper file permissions:
```bash
chmod 644 web/sites/default/settings.php
chmod 755 web/sites/default/files
```

### Security Best Practices

- Keep Drupal core and modules updated
- Use strong passwords and 2FA
- Configure trusted host patterns
- Disable unnecessary modules
- Regular security audits with Drush:
```bash
vendor/bin/drush pm:security
```

## Troubleshooting

### Permission Denied Errors

```bash
chmod -R 775 web/sites/default/files
chown -R www-data:www-data web/sites/default/files
```

### Database Connection Issues

- Verify database credentials in settings.php
- Check database server is running
- Ensure database user has proper privileges

### White Screen of Death

- Check PHP error logs
- Enable error reporting temporarily:
```php
$config['system.logging']['error_level'] = 'verbose';
```

### Cache Issues

```bash
vendor/bin/drush cache:rebuild
```

## Updating Drupal

```bash
# Backup database first
vendor/bin/drush sql:dump > backup.sql

# Update with Composer
composer update drupal/core-recommended --with-dependencies
composer update drupal/core-dev --with-dependencies

# Run database updates
vendor/bin/drush updatedb

# Clear cache
vendor/bin/drush cache:rebuild
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Resources

- [Drupal Documentation](https://www.drupal.org/docs)
- [Drupal API](https://api.drupal.org/)
- [Drupal Community](https://www.drupal.org/community)
- [Composer for Drupal](https://www.drupal.org/docs/develop/using-composer)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Author

Copyright (c) 2025 Saken Tukenov

## Acknowledgments

- Built on Drupal 11 CMS
- Uses Composer for dependency management
- Following Drupal best practices and coding standards
