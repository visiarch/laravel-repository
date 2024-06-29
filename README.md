![laravel Service](https://github.com/visiarch/laravel-service/blob/main/images/laravel-service-banner.png)

# laravel-service

[![Latest Stable Version](http://poser.pugx.org/visiarch/laravel-service/v)](https://packagist.org/packages/visiarch/laravel-service)
[![License](http://poser.pugx.org/visiarch/laravel-service/license)](https://packagist.org/packages/visiarch/laravel-service)

> A Simple Package to create services, using artisan commands in laravel.

This package extends the `make:` commands to help you easily create service classes in Laravel 9+.

# What is Service ?

A service is a component that is responsible for executing an application's business logic. This is a place to place logic that may be too complex or inappropriate to place in the controller.

# Install

```bash
composer require visiarch/laravel-service
```

Once it is installed, you can use any of the commands in your terminal.

# Usage

Services are used to separate business logic from the controller, making the controller leaner and focused on handling HTTP requests and providing appropriate responses.

#### With interface
```bash
php artisan make:service {name} --i
```

#### Without interface
```bash
php artisan make:service {name}
```

# Examples

## Create a service class with interface
```bash
php artisan make:service PostService --i
```
`/app/Interfaces/PostServiceInterface.php`
```php
<?php

namespace App\Interfaces;

/**
 * Interface PostServiceInterface
 *
 * This interface defines the methods for the PostService class.
 */

interface PostServiceInterface
{
   //
}

```


`/app/Services/PostService.php`
```php
<?php

namespace App\Services;

use App\Interfaces\PostServiceInterface;

/**
 * Class UserService
 *
 * @package App\Services
 */
 
class PostService implements PostServiceInterface
{
   // 
}
```

## Create a service class without interface
```bash
php artisan make:service PostService
```




`/app/Services/PostService.php`
```php
<?php

namespace App\Services;

/**
 * Class UserService
 *
 * @package App\Services
 */
 
class PostService
{
   // 
}
```

## Implementation
### With Interface

```php
<?php

namespace App\Interfaces;

use App\Models\Post;

class PostService implements PostServiceInterface
{
    public function store(array $data): Post;
}
```

```php
<?php

namespace App\Services;

use App\Models\Post;

class PostService implements PostServiceInterface
{
    public function store(array $data): Post
    {
        return Post::create($data);
    }
}
```
### Without Interface

```php
<?php

namespace App\Services;

use App\Models\Post;

class PostService
{
    public function store(array $data): Post
    {
        return Post::create($data);
    }
}
```

### How to implement it on the controller?

```php
<?php

namespace App\Services;

use App\Models\Post;
use App\Interfaces\PostServiceInterface;
use App\Http\Requests\PostRequest;

class PostController extends Controller
{
    protected $postService;

    public function __construct(PostServiceInterface $postService)
    {
        $this->postService = $postService;
    }

    public function store(PostRequest $request): RedirectResponse
    {
        $data = $request->validated();
        $this->postService->store($data);

        return redirect()->route('posts.index')->with('success', __('post created'));
    }
}
```
```
Just change PostServiceInterface to PostService if you are implementing without interface
```

## Contributing

Please feel free to fork this package and contribute by submitting a pull request to enhance the functionalities.

## How can I thank you?

Why not star the github repo? I'd love the attention! Why not share the link for this repository on any social media? Spread the word!

Thanks!
visiarch

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
