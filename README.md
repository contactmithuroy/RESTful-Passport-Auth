# Laravel Passport Authentication 
Laravel Passport provides a full OAuth2 server implementation for your Laravel application in a matter of minutes. Passport is built on top of the League OAuth2 server that is maintained by Andy Millington and Simon Hamp.

### You have to just follow a few steps to get following web services
##### Login API
##### Details API




## Getting Started
## Step 2:Install Laravel Passport.

````
composer require laravel/passport

````
## Step 4:Run your database migrations.

````
php artisan migrate

````
## Step 4: Next, you should execute the passport:install Artisan command.

````
php artisan passport:install

````

## Step 5:Add the passport on User Model .

````
use Illuminate\Notifications\Notifiable;
use Laravel\Passport\HasApiTokens;
...
...


class User extends Authenticatable
{
    use HasApiTokens, HasFactory, Notifiable;
  'password',
    ];

````

## Step 6: GO Provider AuthProvider and set.

````
use Laravel\Passport\Passport;
...
...
 public function boot()
    {
       $this->registerPolicies();

        if (! $this->app->routesAreCached()) {
            Passport::routes();
        }
    }

````
## Step 6: GO Config auth.php and set.

````
'api' => [
            'driver' => 'passport',
            'provider' => 'users',
            // 'hash' => false,
        ],

````
## Step 7:Let's create a Controller for userCOntroller

```
php artisan make:controller UserController
````

## Step 8:Now let's Create Registraion and Login function on using UserController

```javascript 
use App\Models\User;
use Illuminate\Support\Facades\Auth;
use Laravel\Passport\HasApiTokens;
use Illuminate\Support\Facades\Hash;
use Validator;
...
...

class UserController extends Controller
{
    public function register(Request $request){
        $rules = [
            'name'=>'required|max:55',
            'email'=>'required|email',
            'password'=>'required',
        ];

        $validator = Validator::make($request->all(),$rules);

        if($validator->fails()){

            return response()->json($validator->errors(),202);

        }else{
            $user = new User();
            $user->name = $request->name;
            $user->email = $request->email;
            $user->password = Hash::make($request->password);
            $user->save();

            $accessToken = $user->createToken('authToken')->accessToken;
            $response = [
                    'user' => $user,
                    'access_token' => $accessToken
                ];

            return response($response, 201);
        }
        
    }

    public function login(Request $request){

        $user= User::where('email', $request->email)->first();

        if (!$user || !Hash::check($request->password, $user->password)) {
            return response([
                'message' => ['These credentials do not match our records.']
            ], 404);
        }
    
        $accessToken = $user->createToken('authToken')->accessToken;
        $response = [
                'user' => $user,
                'access_token' => $accessToken
            ];

        return response($response, 201);
    }
}

````

## Step 9: Now go to api route and create 

```javascript 
use App\Http\Controllers\UserController;
...
...

Route::post('/register',[UserController::class,'registration']);

Route::post('/login',[UserController::class,'login']);


````


## Step 10:  go to postman and register:


```javascript 
....
hit then link
....
http://127.0.0.1:8000/api/register

write on body this json format data
....
{
	"name":"Ethan Hunt",
	"email":"ethan@gmail.com",
	"password":"aaaaaaaa",
	"c_password":"aaaaaaaa"
}

````


## Step 11: Test with postman, Result will be below

```javascript 

{
    "user": {
        "id": 1,
        "name": "Ethan Hunt",
        "email": "ethan@gmail.com",
        "email_verified_at": null,
        "created_at": null,
        "updated_at": null
    },
    "token": "AbQzDgXa..."
}

````

## Step 11: Make Details API or any other with secure route  

```javascript 

Route::middleware('auth:api')->get('/get',[UserController::class,'get']);


````
How to login and get others route
````
Login route always stay outside

Go Hedder in postman set
Authorization and toker Beerer 
[{"key":"Authorization","value":"Bearer 8|8L8XdIE599i3XATYzXTwESON6tLDEeQoXSMKnnFF","description":""}]
and set your url for operation

If any problem with this Operation go bellow and follow the link
https://www.youtube.com/watch?v=j-gF5Qwowy4

Good Luck
Mithu Roy 
````
