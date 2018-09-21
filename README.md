# Apiato Uploader Container
Manage uploading file in Apiato project with dynamic storage driver including amason s3

## Installation
- Copy folder `Uploader`, then paste in in your `apiato` project's container.
- Then run commands
```
composer dumpautoload -o
composer update
php artisan migrate
```

## Usage

### Model
```php
<?php

namespace App\Containers\Product\Models;

use App\Ship\Parents\Models\Model;
use Lloricode\LaravelUploader\Contract\UploaderContract; // <--------  add
use Lloricode\LaravelUploader\UploaderOptions; // <--------  add
use Lloricode\LaravelUploader\Traits\UploaderTrait; // <--------  add

class Product extends Model implements UploaderContract // <--------  add
{
    use UploaderTrait; // <--------  add

    protected $fillable = [

    ];

    protected $attributes = [

    ];

    protected $hidden = [

    ];

    protected $casts = [

    ];

    protected $dates = [
        'created_at',
        'updated_at',
    ];

    /**
     * A resource key to be used by the the JSON API Serializer responses.
     */
    protected $resourceKey = 'products';


    public function uploaderRules() :UploaderOptions // <--------  add
    {
        return UploaderOptions::create()
            ->fileNamePrefix('file-')
            ->disk('local') // any disk in config/filesystems.disk
            ->maxSize(20000000); // byte in decimal = 20mb
    }
}

```

### Action
```php
        $product = Apiato::call('Product@CreateProductTask', [
            
                // product data
            
            ]);
            
        Apiato::call('Uploader@UploaderTask', [
            $product,
            $file, // instance of Illuminate\Http\UploadedFile;
            $product->code // what ever you want its optional
        ]);

```
