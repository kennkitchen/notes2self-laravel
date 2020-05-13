## Starting a Project

`composer create-project --prefer-dist laravel/laravel my-app`

### Adding UI
```
composer require laravel/ui
php artisan ui vue
npm install && npm run dev
php artisan ui vue --auth
```

(You can also use "bootstrap" or "react" instead of "vue".)

### Migrating
Edit your .env file to connect to your database:

```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=mydatabase
DB_USERNAME=mydbuser
DB_PASSWORD=secret
```

Now you can migrate the UI additions:

`php artisan migrate`

