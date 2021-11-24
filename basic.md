## CRUD BASIC IN LARAVEL

### STEP1: INSTALL PROJECT 
> composer create-project laravel/laravel your_project --prefer-dist
### STEP2: CREATE DATABASE & CONFIGURED
Go to <code>phpMyAdmin</code> create new database name and config it in <code>.env</code> file
```
DB_DATABASE=example_db
DB_USERNAME=root
DB_PASSWORD=123
```
### STEP3: CREATE TABLE , MODEL AND CONTROLLER 
1. Create table migration and model 
> php artisan make:model <code>Example</code> -m 
2. Create Controller 
> php artisan make:controller <code>ExampleController </code> --api

### STEP4: ADD NEW COLUMN TO TABALE
```
/**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('examples', function (Blueprint $table) {
            $table->id();
            $table->string('title');
            $table->text('description');
            $table->timestamps();
        });
    }
```

### STEP5: MODELS
```
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Example extends Model
{
    use HasFactory;

    protected $fillable = [
        'title',
        'description'
    ];

    protected $hidden = [
        'created_at',
        'updated_at'
    ];
}

```
### STEP5: CONTROLLERS
1. Import the model to controller
```
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Models\Example;

class BookController extends Controller
{
```
2. index function 
```
/**
     * Display a listing of the resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function index()
    {
        return Example::latest()->get();
    }

```
3. store function
```
/**
     * Store a newly created resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @return \Illuminate\Http\Response
     */
    public function store(Request $request)
    {
        $example = new Example();
        $example->title = $request->title;
        $example->description = $request->description;
        $example->save();

        return response()->json(['message' => 'example created successfully'], 201);
    }
```
4. show function
```

    /**
     * Display the specified resource.
     *
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function show($id)
    {
        return Example::findOrFail($id);
    }

```

5. update function
```
/**
     * Update the specified resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function update(Request $request, $id)
    {
        $example = Example::findOrFail($id);
        $example->title = $request->title;
        $example->description = $request->description;
        $example->save();

        return response()->json(['message' => 'example updated successfully'], 200);
    }
```
6. destroy function
```
    /**
     * Remove the specified resource from storage.
     *
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function deleteBook($id)
    {
        return Book::destroy($id);
    }
```
### STEP6: ROUTE
```
<?php

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;

use App\Http\Controllers\BookController;
use App\Http\Controllers\CountryController;

Route::get('examples', [BookController::class, 'index']);
Route::get('examples/{id}', [BookController::class, 'show']);

Route::post('examples', [BookController::class, 'store']);
Route::put('examples/{id}', [BookController::class, 'update']);
Route::delete('examples/{id}', [BookController::class, 'destroy']);


```

### STEP7: MIGRATION
To migrate your table to database you need to run the following command:
> php artisan migrate
