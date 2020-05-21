# LARAVEL API PROJECT

Criar uma API REST com Laravel

Seguindo o tutorial: https://www.twilio.com/blog/building-and-consuming-a-restful-api-in-laravel-php

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

6. Criar as rotas no arqivo api.php pois ai a rota fica /api/nome_rota

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

8. Retornar dados 

        $students = Student::get()->toJson(JSON_PRETTY_PRINT);
        return response($students, 200);

9. Fazendo retornar pelo id

          public function getStudent($id) {
            if (Student::where('id', $id)->exists()) {
                $student = Student::where('id', $id)->get()->toJson(JSON_PRETTY_PRINT);
                return response($student, 200);
              } else {
                return response()->json([
                  "message" => "Student not found"
                ], 404);
              }
          }

10. Fazendo o Update de um estudante

        if (Student::where('id', $id)->exists()) {
            $student = Student::find($id);
            $student->name = is_null($request->name) ? $student->name : $request->name;
            $student->course = is_null($request->course) ? $student->course : $request->course;
            $student->save();

            return response()->json([
                "message" => "records updated successfully"
            ], 200);
            } else {
            return response()->json([
                "message" => "Student not found"
            ], 404);

        }

11. Deletando o estudante

          if(Student::where('id', $id)->exists()) {
            $student = Student::find($id);
            $student->delete();

            return response()->json([
              "message" => "records deleted"
            ], 202);
          } else {
            return response()->json([
              "message" => "Student not found"
            ], 404);
          }
          
Com isso podemos concluir que:

# API, REST e RESTFUL

## API

Cliente (Client)
Garçom (pedidos, levar seus pedidos, para a cozinha) (API)
Cozinha (Server)

Acrônimo de Application Programming Interface (Interface de Programação de Aplicações) é basicamente um conjunto de rotinas e padrões estabelecidos por uma aplicação, para que outras aplicações possam utilizar as funcionalidades desta aplicação.

- Responsável por estabelecer comunicação entre diferentes serviços.
- Meio de campo entre as tecnologias.
- Intermediador para troca de informações.

## REST

um acrônimo para REpresentational State Transfer (Transferência de Estado Representativo).

Será feita a transferência de dados de uma maneira simbólica, figurativa, representativa, de maneira didática.

A transferência de dados, geralmente, usando o protocolo HTTP.

O REST delimita algumas obrigações nessas transferências de dados.

Resources seria então: Uma entidade ou um objeto.

### 6 NECESSIDADES (constraints) para ser RESTful

- _Uniform Interface_: Manter uma uniformidade, uma constância, um padrão na construção da interface. Nossa API precisa ser coerente para quem vai consumi-lá. Precisa fazer sentido para o cliente e não ser confusa. Logo, coisas como: o uso correto dos verbos HTTP; endpoints coerentes (todos os endpoints no plural, por exemplo); usar somente uma linguagem de comunicação (json) e não várias ao mesmo tempo; sempre enviar respostas aos clientes; são exemplos de aplicação de uma interface uniforme.

- _Client-server_: Separação do cliente e do armazenamento de dados (servidor), dessa forma, poderemos ter uma portabilidade do nosso sistema, usando o React para WEB e React Native para o smartphone, por exemplo.

- _Stateless_: Cada requisição que o cliente faz para o servidor, deverá conter todas as informações necessárias para o servidor entender e responder (RESPONSE) a requisição (REQUEST). Exemplo: A sessão do usuário deverá ser enviada em todas as requisições, para saber se aquele usuário está autenticado e apto a usar os serviços, e o servidor não pode lembrar que o cliente foi autenticado na requisição anterior. Nos nossos cursos, temos por padrão usar tokens para as comunicações.

- _Cacheable_: As respostas para uma requisição, deverão ser explicitas ao dizer se aquela resquição, pode ou não ser cacheada pelo cliente.

- _Layered System_: O cliente acessa a um endpoint, sem precisar saber da complexidade, de quais passos estão sendo necessários para o servidor responder a requisição, ou quais outras camadas o servidor estará lidando, para que a requisição seja respondida.

- _Code on demand (optional)_: Dá a possibilidade da nossa aplicação pegar códigos, como o javascript, por exemplo, e executar no cliente.

## RESTFUL

RESTful, é a aplicação dos padrões REST.

## BOAS PRÁTICAS

- Utilizar verbos HTTP para nossas requisições.
- Utilizar plural ou singular na criação dos endpoints? _NÃO IMPORTA!_ use um padrão!!
- Não deixar barra no final do endpoint
- Nunca deixe o cliente sem resposta!

### VERBOS HTTP

- GET: Receber dados de um Resource.
- POST: Enviar dados ou informações para serem processados por um Resource.
- PUT: Atualizar dados de um Resource.
- DELETE: Deletar um Resource

### STATUS DAS RESPOSTAS

- 1xx: Informação
- 2xx: Sucesso
  - 200: OK
  - 201: CREATED
  - 204: Não tem conteúdo PUT POST DELETE
- 3xx: Redirection
- 4xx: Client Error
  - 400: Bad Request
  - 404: Not Found!
- 5xx: Server Error
  500: Internal Server Error

Tirado do rocketseat!

## Métodos de requisição HTTP

GET
O método GET solicita a representação de um recurso específico. Requisições utilizando o método GET devem retornar apenas dados.

HEAD
O método HEAD solicita uma resposta de forma idêntica ao método GET, porém sem conter o corpo da resposta.
 
POST
O método POST é utilizado para submeter uma entidade a um recurso específico, frequentemente causando uma mudança no estado do recurso ou efeitos colaterais no servidor.

PUT
O método PUT substitui todas as atuais representações do recurso de destino pela carga de dados da requisição.

DELETE
O método DELETE remove um recurso específico.

CONNECT
O método CONNECT estabelece um túnel para o servidor identificado pelo recurso de destino.

OPTIONS
O método OPTIONS é usado para descrever as opções de comunicação com o recurso de destino.

TRACE
O método TRACE executa um teste de chamada loop-back junto com o caminho para o recurso de destino.

PATCH
O método PATCH é utilizado para aplicar modificações parciais em um recurso.
