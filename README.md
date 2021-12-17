# Lumen PHP Framework

[![Build Status](https://travis-ci.org/laravel/lumen-framework.svg)](https://travis-ci.org/laravel/lumen-framework)
[![Total Downloads](https://img.shields.io/packagist/dt/laravel/framework)](https://packagist.org/packages/laravel/lumen-framework)
[![Latest Stable Version](https://img.shields.io/packagist/v/laravel/framework)](https://packagist.org/packages/laravel/lumen-framework)
[![License](https://img.shields.io/packagist/l/laravel/framework)](https://packagist.org/packages/laravel/lumen-framework)

Laravel Lumen is a stunningly fast PHP micro-framework for building web applications with expressive, elegant syntax. We believe development must be an enjoyable, creative experience to be truly fulfilling. Lumen attempts to take the pain out of development by easing common tasks used in the majority of web projects, such as routing, database abstraction, queueing, and caching.

## Official Documentation

Documentation for the framework can be found on the [Lumen website](https://lumen.laravel.com/docs).

## Contributing

Thank you for considering contributing to Lumen! The contribution guide can be found in the [Laravel documentation](https://laravel.com/docs/contributions).

## Security Vulnerabilities

If you discover a security vulnerability within Lumen, please send an e-mail to Taylor Otwell at taylor@laravel.com. All security vulnerabilities will be promptly addressed.

## License

The Lumen framework is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).

##  Create and run Lumen project
- Create Lumen project: **composer create-project --prefer-dist laravel/lumen blog**
- Run Lumen project: **php -S localhost:8000 -t public**

##  Config Lumen for work
By default, artisan commands are not available in the lumen framework. We will have to install artisan command 
individually. 

To use some generators command in Lumen (just like you do in Laravel), you need to add this package:

[flipbox/lumen-generator](https://github.com/flipboxstudio/lumen-generator)

##CRUD Operation in Lumen
- Open .env file and add database configuration
- add APP_KEY by run: **php artisan key:generate**
- For CRUD operation in blog post we will need the following files
    * Model
    * Migration
    * Factory and
    * Controller
    
  Create all the file with a single command **php artisan make:model Post -fmc**
 - Open "database/migrations/create_post_table" and add the following code
     ```public function up()
            {
                Schema::create('posts', function (Blueprint $table) {
                    $table->id();
                    $table->string('title');
                    $table->text('body');
                    $table->timestamps();
                });
            }
    ```
 - For migrate database run command **php artisan migrate**
 - Now add some dummy data inside **posts** table
    * Open **PostFactory.php** class "lumen_rest_api\database\factories\PostFactory.php" and write the following code
      ```
      <?php       
           namespace Database\Factories;
           
           use App\Models\Post;
           use Illuminate\Database\Eloquent\Factories\Factory;
           
           class PostFactory extends Factory
           {
               protected $model = Post::class;
           
               public function definition(): array
               {
                return [
                    'title' => $this->faker->sentence,
                    'body' => $this->faker->paragraph,
                ];
               }
           }
      ```
   * Open **Post.php** class "app\Models\Post.php" and write the following code
     ```
     <?php       
       namespace App\Models;
       
       use Illuminate\Database\Eloquent\Factories\HasFactory;
       use Illuminate\Database\Eloquent\Model;
       
       class Post extends Model
       {
           use HasFactory;
       }
     ```
   * Let's run tinker and create 10 dummy post inside **posts** table
       
   ```
   php artisan tinker
   App\Models\Post::factory()->count(10)->create()
   ```
- Now create a route to **GET** all posts
    * Open **web.php** file inside the directory "lumen_rest_api\routes\web.php"
    
      ```
        <?php
         /** @var \Laravel\Lumen\Routing\Router $router */
         
         /*
         |--------------------------------------------------------------------------
         | Application Routes
         |--------------------------------------------------------------------------
         |
         | Here is where you can register all of the routes for an application.
         | It is a breeze. Simply tell Lumen the URIs it should respond to
         | and give it the Closure to call when that URI is requested.
         |
         */
         
         $router->get('/', function () use ($router) {
             return $router->app->version();
         });
         
         $router->group(['prefix' => 'api'], function () use ($router){
             $router->get('/posts', 'PostController@index');
         });
      ```
    * Create index() method in **PostController.php** class inside the directory "app\Http\Controllers\PostController.php"
      ```
      <?php
       namespace App\Http\Controllers;
       
       use App\Models\Post;
       use Illuminate\Http\Request;
       
       class PostController extends Controller
       {
           public function index(){
               return Post::all();
           }
       }
        ```
  * Now for use **Eloquent** uncomment the following line inside **app.php** in the path "lumen_rest_api\bootstrap\app.php"
  ```$app->withEloquent();```
  
  * Run lumen project by run the command: **php -S lumen_rest_api.test:8000 -t public**
  * Now open postman and check the endpoints [http://lumen_rest_api.test:8000/api/posts](http://lumen_rest_api.test:8000/)
  * We will show the following JSON file
      ```
      [
             {
                 "id": 1,
                 "title": "Autem sint ullam et enim illum dolores.",
                 "body": "Cumque voluptatem aut enim. Iste esse distinctio id itaque natus omnis. Corporis sed excepturi id error quo. Ullam reiciendis fuga quo eos sint.",
                 "created_at": "2021-12-16T20:02:39.000000Z",
                 "updated_at": "2021-12-16T20:02:39.000000Z"
             },
             {
                 "id": 2,
                 "title": "Voluptates quasi neque asperiores ratione.",
                 "body": "Qui ab dolorem officiis saepe veniam voluptas culpa ab. Et qui voluptas quasi enim. Odio et laudantium omnis.",
                 "created_at": "2021-12-16T20:02:39.000000Z",
                 "updated_at": "2021-12-16T20:02:39.000000Z"
             },
             {
                 "id": 3,
                 "title": "Ut qui suscipit consequuntur excepturi voluptatem excepturi.",
                 "body": "Asperiores recusandae eos cum aliquam minus quod sit. Labore omnis ratione harum culpa quasi fugit corporis. Et maiores sint numquam sed et.",
                 "created_at": "2021-12-16T20:02:39.000000Z",
                 "updated_at": "2021-12-16T20:02:39.000000Z"
             },
             {
                 "id": 4,
                 "title": "Asperiores qui qui qui qui.",
                 "body": "Possimus quae quia laudantium eum itaque et nisi nihil. Autem non optio tenetur. Consectetur consequuntur nesciunt eligendi velit qui.",
                 "created_at": "2021-12-16T20:02:39.000000Z",
                 "updated_at": "2021-12-16T20:02:39.000000Z"
             },
             {
                 "id": 5,
                 "title": "Amet ab enim qui nemo tempora error et.",
                 "body": "Similique libero voluptatum veniam suscipit. Pariatur qui deserunt consequatur in. Illum nihil pariatur officiis voluptas molestiae et cumque autem.",
                 "created_at": "2021-12-16T20:02:39.000000Z",
                 "updated_at": "2021-12-16T20:02:39.000000Z"
             },
             {
                 "id": 6,
                 "title": "Accusamus voluptates architecto magni earum ad explicabo possimus.",
                 "body": "Explicabo molestias et qui praesentium. Error nobis enim id laborum sed. Nihil explicabo perspiciatis quia tempore.",
                 "created_at": "2021-12-16T20:02:39.000000Z",
                 "updated_at": "2021-12-16T20:02:39.000000Z"
             },
             {
                 "id": 7,
                 "title": "Esse facilis vel odio omnis deserunt.",
                 "body": "Eveniet est magnam totam. Natus architecto eum sit veniam dolores maiores. Non voluptatem possimus at quo ratione labore adipisci consequatur. Labore voluptas porro inventore perferendis aut veritatis.",
                 "created_at": "2021-12-16T20:02:39.000000Z",
                 "updated_at": "2021-12-16T20:02:39.000000Z"
             },
             {
                 "id": 8,
                 "title": "Quas nihil laudantium qui quam iste amet beatae.",
                 "body": "Ullam totam adipisci dolorum dolore. Pariatur laudantium voluptatem saepe est illo labore. Totam tempore pariatur in officiis dolores.",
                 "created_at": "2021-12-16T20:02:39.000000Z",
                 "updated_at": "2021-12-16T20:02:39.000000Z"
             },
             {
                 "id": 9,
                 "title": "Velit ea a unde ut id.",
                 "body": "Delectus animi voluptas voluptatum et unde. Libero est ut et adipisci sapiente praesentium. Cumque eos reiciendis et totam tenetur nostrum. Quia in ex nam odio.",
                 "created_at": "2021-12-16T20:02:39.000000Z",
                 "updated_at": "2021-12-16T20:02:39.000000Z"
             },
             {
                 "id": 10,
                 "title": "Ducimus culpa voluptas quia velit minima.",
                 "body": "Rerum quia culpa soluta iste laboriosam. Veniam ab provident sed voluptas inventore distinctio et. Sed est sint qui voluptatum ipsa rerum officia eveniet. Sed illo laboriosam ad unde ratione eum enim.",
                 "created_at": "2021-12-16T20:02:39.000000Z",
                 "updated_at": "2021-12-16T20:02:39.000000Z"
             }
         ]
      ```
- Now create a route for **CREATE** a new post
    * Inside **web.php** add the following route
      ```
      $router->post('/post', 'PostController@store');
      ```
    * Open **PostController.php** and add the following code
      ```
      //create a new post
      public function store(Request $request)
      {
          try {
              $post = new Post();
              $post->title = $request->title;
              $post->body = $request->body;
    
              if ($post->save()) {
                  return response()->json(['status_code' => '200', 'status' => 'success', 'message' => 'Post created successfully']);
              }
    
          } catch (\Exception $e) {
              return response()->json(['status' => 'error', 'message' => $e->getMessage()]);
          }
      }
      ```
    * Open **Postman** and create a new **POST** request
    
      endpoint: [http://lumen_rest_api.test:8000/api/post](http://lumen_rest_api.test:8000/api/post)
      ```
      {
          "title": "new post created by ruhul on 16-12-2021",
          "body": "This is the first create request using laravel-lumen framework"
      }
      ```
      **Response:**
      ```
      {
          "status_code": "200",
          "status": "success",
          "message": "Post created successfully"
      }
      ```
- Now create another route for UPDATE an existing post
    * Inside **web.php** add the following route
      ```
      $router->put('/post/{id}', 'PostController@update');
      ```
    * Open **PostController.php** and add the following code
      ```
      //update an existing post
      public function update(Request $request, $id){
          try {
              $post = Post::findOrFail($id);
              $post->title = $request->title;
              $post->body = $request->body;
        
              if ($post->save()) {
                  return response()->json(['status' => 'success', 'message' => 'Post updated successfully']);
              }
        
          } catch (\Exception $e) {
              return response()->json(['status' => 'error', 'message' => $e->getMessage()]);
          }
      }
      ```
    * Open **Postman** and create a new **POST** request
        
      endpoint: [http://lumen_rest_api.test:8000/api/post/13](http://lumen_rest_api.test:8000/api/post/13)
      ```
      {
          "title": "First UPDATE POST is created in laravel-lumen",
          "body": "Laravel-lumen is best suitable for API development"
      }
      ```
      **Response:**
      ```
      {
          "status": "success",
          "message": "Post updated successfully"
      }
      ```
      
- Now create another route for DELETE an existing post
    * Inside **web.php** add the following route
      ```
      $router->delete('/post/{id}', 'PostController@destroy');
      ```
    * Open **PostController.php** and add the following code
      ```
      //delete an existing post
      public function destroy($id){
          try {
              $post = Post::findOrFail($id);
  
              if ($post->delete()) {
                  return response()->json(['status' => 'success', 'message' => 'Post deleted successfully']);
              }
  
          } catch (\Exception $e) {
              return response()->json(['status' => 'error', 'message' => $e->getMessage()]);
          }
      }
      ```
    * Open **Postman** and create a new **DELETE** request
        
      endpoint: [http://lumen_rest_api.test:8000/api/post/13](http://lumen_rest_api.test:8000/api/post/13)
      
      Here no need to body data
      
      **Response:**
      ```
      {
          "status": "success",
          "message": "Post deleted successfully"
      }
      ```
- Start from 
    
    **Time: 12.24**
