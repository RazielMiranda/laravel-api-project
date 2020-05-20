# LARAVEL API PROJECT

Criar uma API REST com Laravel

seguindo o tutorial: https://www.twilio.com/blog/building-and-consuming-a-restful-api-in-laravel-php

1. Criar o APP e o banco de dados

2. Criar a model

       php artisan make:model Student -m

3. Editar a model para aceitar os padroes

    <?php

        namespace App;

        use Illuminate\Database\Eloquent\Model;

        class Student extends Model
        {
            protected $table = 'students';

            protected $fillable = ['name', 'course'];
        }

4. Rodar as migrations

        Schema::create('students', function (Blueprint $table) {
            $table->increments('id');
            $table->string('name');
            $table->string('course');
            $table->timestamps();
        });

        php artisan migrate

5. Criar as rotas e controller

        php artisan make:controller ApiController

    public function getAllStudents() {
      // logic to get all students goes here
    }

    public function createStudent(Request $request) {
      // logic to create a student record goes here
    }

    public function getStudent($id) {
      // logic to get a student record goes here
    }

    public function updateStudent(Request $request, $id) {
      // logic to update a student record goes here
    }

    public function deleteStudent ($id) {
      // logic to delete a student record goes here
    }

6. Criar as rotas

    Route::get('students', 'ApiController@getAllStudents');
    Route::get('students/{id}', 'ApiController@getStudent');
    Route::post('students, 'ApiController@createStudent');
    Route::put('students/{id}', 'ApiController@updateStudent');
    Route::delete('students/{id}','ApiController@deleteStudent');

7. No controller criar o metodo de criação de estudante

  public function createStudent(Request $request) {
    $student = new Student;
    $student->name = $request->name;
    $student->course = $request->course;
    $student->save();

    return response()->json([
        "message" => "student record created"
    ], 201);
  }

8. Testar com o POSTMAN

9. Retornar dados

    $students = Student::get()->toJson(JSON_PRETTY_PRINT);
    return response($students, 200);
    


