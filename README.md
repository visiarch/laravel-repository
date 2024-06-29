![laravel Repository](https://github.com/visiarch/laravel-repository/blob/main/images/laravel-repository-banner.png)

# laravel-repository

[![Latest Stable Version](http://poser.pugx.org/visiarch/laravel-repository/v)](https://packagist.org/packages/visiarch/laravel-repository)
[![License](http://poser.pugx.org/visiarch/laravel-repository/license)](https://packagist.org/packages/visiarch/laravel-repository)

[!["Buy Me A Coffee"](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)](https://www.buymeacoffee.com/bagussuandana)

> A Simple Package to create repositories, using artisan commands in laravel.

This package extends the `make:` commands to help you easily create repository classes in Laravel 9+.

# What is Repository ?

Repository is a design pattern used to manage data access logic. It provides abstraction between the application and data access layers, such as databases.

# Install

```bash
composer require visiarch/laravel-repository
```

Once it is installed, you can use any of the commands in your terminal.

# Usage

Repositories are used to separate data access logic from business logic, allowing developers to change how data is stored and retrieved without affecting business logic.

#### With interface

```bash
php artisan make:repository {name} --i
```

#### Without interface

```bash
php artisan make:repository {name}
```

# Examples

## Create a repository class with interface

```bash
php artisan make:repository PostRepository --i
```

`/app/Interfaces/PostRepositoryInterface.php`

```php
<?php

namespace App\Interfaces;

/**
 * Interface PostRepositoryInterface
 *
 * This interface defines the methods for the PostRepository class.
 */

interface PostRepositoryInterface
{
   //
}

```

`/app/Repositories/PostRepository.php`

```php
<?php

namespace App\Repositories;

use App\Interfaces\PostRepositoryInterface;

/**
 * Class PostRepository
 *
 * @package App\Repositories
 */

class PostRepository implements PostRepositoryInterface
{
   //
}
```

## Create a repository class without interface

```bash
php artisan make:repository PostRepository
```

`/app/Repositories/PostRepository.php`

```php
<?php

namespace App\Repositories;

/**
 * Class PostRepository
 *
 * @package App\Repositories
 */

class PostRepository
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

class PostRepository implements PostRepositoryInterface
{
    public function store(array $data): Post;
}
```

```php
<?php

namespace App\Repositories;

use App\Models\Post;

class PostRepository implements PostRepositoryInterface
{
    protected $post;

    public function __construct(Post $post){
        $this->post = $post;
    }

    public function store(array $data): Post
    {
        return $this->post->create($data);
    }
}
```

### Without Interface

```php
<?php

namespace App\Repositories;

use App\Models\Post;

class PostRepository
{
    protected $post;

    public function __construct(Post $post){
        $this->post = $post;
    }

    public function store(array $data): Post
    {
        return $this->post->create($data);
    }
}
```

### How to implement it on the controller?

```php
<?php

namespace App\Repositories;

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
Just change PostRepositoryInterface to PostRepository if you are implementing without interface
```

## Contributing

Please feel free to fork this package and contribute by submitting a pull request to enhance the functionalities.

## How can I thank you?

Why not star the github repo? I'd love the attention! Why not share the link for this repository on any social media? Spread the word!

Thanks!
visiarch

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
