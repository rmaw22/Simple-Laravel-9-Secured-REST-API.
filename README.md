# Simple-Laravel-9-Secured-REST-API.
Created by : Rizky Milan 

# Step 1 => Installation 

## Laravel Package Development

[![https://phppackagedevelopment.com](https://beyondco.de/courses/phppd.jpg)](https://phppackagedevelopment.com)

If you want to learn how to create reusable PHP packages yourself, take a look at my upcoming [PHP Package Development](https://phppackagedevelopment.com) video course.
## Tech Requirement
- PHP 8.0.2
- MySql
- Composer

## Installation

You can install the package via composer:

```bash
composer create-project laravel/laravel first-laravel9
```
Now GO to the Installed Directory

```bash
cd first-laravel9
```

Next to serve application
```bash
php artisan serve
```
## Step - 2
Create a Model Post with migration.
```bash
php artisan make:model Post -m
```
Next Update The Migration file at ```database/migrations``` folder.
```bash
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('posts', function (Blueprint $table) {
            $table->id();
            $table->string('title');
            $table->longText('description');
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('posts');
    }
};

```
Next update the Model fillable property in ```app/models/Post.php```
```bash
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Post extends Model
{
    use HasFactory;

    protected $fillable = ['title', 'description'];
}
```
## Step - 3
Now Generate controller by running command
```bash
php artisan make:controller Api\\PostController --model=Post
```
this command will generate the file in 
```app/Http/Controllers/Api/PostController.php```
Open the file and update the code below.

```bash
<?php

namespace App\Http\Controllers\Api;

use App\Http\Controllers\Controller;
use App\Http\Requests\StorePostRequest;
use App\Models\Post;
use Illuminate\Http\Request;

class PostController extends Controller
{
    /**
     * Display a listing of the resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function index()
    {
        $posts = Post::all();

        return response()->json([
            'status' => true,
            'posts' => $posts
        ]);
    }

    /**
     * Show the form for creating a new resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function create()
    {
        //
    }

    /**
     * Store a newly created resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @return \Illuminate\Http\Response
     */
    public function store(StorePostRequest $request)
    {
        $post = Post::create($request->all());

        return response()->json([
            'status' => true,
            'message' => "Post Created successfully!",
            'post' => $post
        ], 200);
    }

    /**
     * Display the specified resource.
     *
     * @param  \App\Models\Post  $post
     * @return \Illuminate\Http\Response
     */
    public function show(Post $post)
    {
        //
    }

    /**
     * Show the form for editing the specified resource.
     *
     * @param  \App\Models\Post  $post
     * @return \Illuminate\Http\Response
     */
    public function edit(Post $post)
    {
        //
    }

    /**
     * Update the specified resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \App\Models\Post  $post
     * @return \Illuminate\Http\Response
     */
    public function update(StorePostRequest $request, Post $post)
    {
        $post->update($request->all());

        return response()->json([
            'status' => true,
            'message' => "Post Updated successfully!",
            'post' => $post
        ], 200);
    }

    /**
     * Remove the specified resource from storage.
     *
     * @param  \App\Models\Post  $post
     * @return \Illuminate\Http\Response
     */
    public function destroy(Post $post)
    {
        $post->delete();

        return response()->json([
            'status' => true,
            'message' => "Post Deleted successfully!",
        ], 200);
    }
}
```
## Step 4
Now Let's create the request to validate the data by running command below.
```bash
php artisan make:request StorePostRequest
```
Now open file from ```app/Http/Requests/StorePostRequest.php``` and update the code below.
```bash
<?php

namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;

class StorePostRequest extends FormRequest
{
    /**
     * Determine if the user is authorized to make this request.
     *
     * @return bool
     */
    public function authorize()
    {
        return true;
    }

    /**
     * Get the validation rules that apply to the request.
     *
     * @return array
     */
    public function rules()
    {
        return [
            "title" => "required|max:70",
            "description" => "required"
        ];
    }
}
```

## Step - 5
Now create the API routes in ```routes/api.php```

```bash
<?php

use App\Http\Controllers\Api\PostController;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;


Route::apiResource('posts', PostController::class);
```
Now serve the application and open the URL in postman.
The results will look like.
![Result](https://res.cloudinary.com/practicaldev/image/fetch/s--5SIfC_Sh--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8r3vfimiyhn7hk5loxw4.png)

![Result](https://res.cloudinary.com/practicaldev/image/fetch/s--72-g2utb--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/zu9lvdy0u0c9at730t2j.png)

![Result](https://res.cloudinary.com/practicaldev/image/fetch/s--xo9meXzk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/yfxbjk1rvlo4u3znukct.png)

![Result](https://res.cloudinary.com/practicaldev/image/fetch/s--jtGjYHGU--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/iqkbxyxfmptkxojrjg95.png)

## Make REST API AUTHENTICATION in LARAVEL 9 USING LARAVEL SANCTUM
Laravel Sanctum provides a featherweight authentication system for SPAs (single page applications), mobile applications, and simple, token based APIs.

## Installation Steps
If you are not using LARAVEL 9 you need to install LARAVEL Sanctum Otherwise you can skip the installation step.

## Step 1
Install via composer
```bash
composer require laravel/sanctum
```
## Step 2
Publish the Sanctum Service Provider
```bash
php artisan vendor:publish --provider="Laravel\Sanctum\SanctumServiceProvider"
```

## Step 3
Migrate The Database
```bash
php artisan migrate
```

## USING SANCTUM IN LARAVEL
User HasApiTokens Trait in App\Models\User
In Order to use Sanctum we need to use ```HasApiTokens``` Trait Class in User Model.
User Model should look like.
```bash
<?php

namespace App\Models;

use Illuminate\Contracts\Auth\MustVerifyEmail;
use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Notifications\Notifiable;
use Laravel\Sanctum\HasApiTokens;

class User extends Authenticatable
{
    use HasApiTokens, HasFactory, Notifiable;

    /**
     * The attributes that are mass assignable.
     *
     * @var array<int, string>
     */
    protected $fillable = [
        'name',
        'email',
        'password',
    ];

    /**
     * The attributes that should be hidden for serialization.
     *
     * @var array<int, string>
     */
    protected $hidden = [
        'password',
        'remember_token',
    ];

    /**
     * The attributes that should be cast.
     *
     * @var array<string, string>
     */
    protected $casts = [
        'email_verified_at' => 'datetime',
    ];
}
```
## API Authentication Routes
Create AuthController to handle all authentication realted to API
```bash
php artisan make:controller Api\\AuthController
```
In ```routes\api.php file``` update the API
```bash
Route::post('/auth/register', [AuthController::class, 'createUser']);
Route::post('/auth/login', [AuthController::class, 'loginUser']);
```
Now update ```AuthContoller``` with

```bash
<?php

namespace App\Http\Controllers\Api;

use App\Models\User;
use Illuminate\Http\Request;
use App\Http\Controllers\Controller;
use Illuminate\Support\Facades\Auth;
use Illuminate\Support\Facades\Hash;
use Illuminate\Support\Facades\Validator;

class AuthController extends Controller
{
    /**
     * Create User
     * @param Request $request
     * @return User 
     */
    public function createUser(Request $request)
    {
        try {
            //Validated
            $validateUser = Validator::make($request->all(), 
            [
                'name' => 'required',
                'email' => 'required|email|unique:users,email',
                'password' => 'required'
            ]);

            if($validateUser->fails()){
                return response()->json([
                    'status' => false,
                    'message' => 'validation error',
                    'errors' => $validateUser->errors()
                ], 401);
            }

            $user = User::create([
                'name' => $request->name,
                'email' => $request->email,
                'password' => Hash::make($request->password)
            ]);

            return response()->json([
                'status' => true,
                'message' => 'User Created Successfully',
                'token' => $user->createToken("API TOKEN")->plainTextToken
            ], 200);

        } catch (\Throwable $th) {
            return response()->json([
                'status' => false,
                'message' => $th->getMessage()
            ], 500);
        }
    }

    /**
     * Login The User
     * @param Request $request
     * @return User
     */
    public function loginUser(Request $request)
    {
        try {
            $validateUser = Validator::make($request->all(), 
            [
                'email' => 'required|email',
                'password' => 'required'
            ]);

            if($validateUser->fails()){
                return response()->json([
                    'status' => false,
                    'message' => 'validation error',
                    'errors' => $validateUser->errors()
                ], 401);
            }

            if(!Auth::attempt($request->only(['email', 'password']))){
                return response()->json([
                    'status' => false,
                    'message' => 'Email & Password does not match with our record.',
                ], 401);
            }

            $user = User::where('email', $request->email)->first();

            return response()->json([
                'status' => true,
                'message' => 'User Logged In Successfully',
                'token' => $user->createToken("API TOKEN")->plainTextToken
            ], 200);

        } catch (\Throwable $th) {
            return response()->json([
                'status' => false,
                'message' => $th->getMessage()
            ], 500);
        }
    }
}
```
## Protect API With Authentication we need to use ```auth:sanctum``` middleware.
```bash
Route::apiResource('posts', PostController::class)->middleware('auth:sanctum');
```
Here are the results.

![Result](https://res.cloudinary.com/practicaldev/image/fetch/s--nOROIoxz--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/j3payp41pzmr2lcuuuvx.png)
![Result](https://res.cloudinary.com/practicaldev/image/fetch/s--B1tTvRSO--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/685yuqku1c8mz1xcaimc.png)
![Result](https://res.cloudinary.com/practicaldev/image/fetch/s--s8ncoClG--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/up6yc28lo9j3vccl5r7y.png)

### Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information what has changed recently.

## Source Information
```https://dev.to/techtoolindia/getting-started-with-laravel-9-secured-rest-api-390g```
## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

### Security

If you discover any security related issues, please email marcel@beyondco.de instead of using the issue tracker.

## Credits

- [Marcel Pociot](https://github.com/mpociot)
- [All Contributors](../../contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
