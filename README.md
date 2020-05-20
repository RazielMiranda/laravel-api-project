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

4. 

