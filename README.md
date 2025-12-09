# CRUD-em-Laravel
Um passo a passo de Crud feito á mão

Esquema CRUD em Laravel:

Model - > 

$protected $fillable = ['info1','info2'];

Migrations ->

Schema::create('nomeTabela', function (Blueprint $table){
$table->id();
$table->string('info1')
$table->decimal('info2', 8, 2) Pra ficar em decimal
});

View ->

Pasta
Index.blade
Create.blade
Edit.blade

Controller ->
 public function index(){
 	$parametros = NomeDaModel::all(); // Busca todos os valores no banco de dados
        return view('Pasta.index', ['parametros' => $parametros ]);
}

 public function create(){
 return view('games.create');
}

 public function store(Request $request){
  NomeDaModel::create([
            'info1' => $request->info1,
            'info2' => $request->info2,
        	    ]);
	
        return redirect()->route('Pasta.index');
}

public function edit(NomeDaModel $parametro){
return view("Pasta.edit", compact("parametro"));
}

 public function Update (Request $request, NomeDaModel $parametro){
  $parametro::upadate([
            'info1' => $request->info1,
            'info2' => $request->info2,
        ]);
	return redirect("/Pasta");
}
 public function delete(NomeDaModel $parametro){
 $parametro->delete();
        return redirect("/Pasta");
}

crio as Rotas ->

Route::get('/Pasta', [Controller::class, 'index'])->name('Pasta.index');
Route::get('/Pasta/create', [Controller::class, 'create'])->name('Pasta.create');
Route::post('/Pasta', [Controller::class, 'store'])->name('Pasta.store');
Route::get('/Pasta/{game}/edit', [Controller::class, 'edit'])->name('Pasta.edit');
Route::put('/Pasta/{game}', [Controller::class, 'update'])->name('Pasta.update');
Route::delete('/Pasta/{game}', [Controller::class, 'destroy'])->name('Pasta.destroy');

Route::resource('/Pasta', Controller::class);


Views -> 
INDEX
   <h1>Lista de Usuários</h1>
<a href="{{ route('Users.create') }}">Criar novo usuario</a>
<div class="list">
    @foreach ($usuarios as $usuario)
    <div class="user-card">
    Nome do Usuario
        <h4>{{ $usuario->nome }}</h4>
        <br>
        Email do Usuario
        <h4>{{ $usuario->email }}</h4>
        <br>
        Data de Nascimento do Usuario
        <h4>{{ $usuario->data_nascimento }}</h4>
        <br>
        Jogo Favorito do Usuario
        <h4>{{ $usuario->jogo_favorito }}</h4>
        <br>
        <a href="Users/{{ $usuario->id }}/edit">Editar</a>
        <form action="{{ route('Users.destroy', $usuario->id) }}" method="POST">
            @csrf
            @method('DELETE')
            <button>Deletar Usuario </button>
        </div>
    @endforeach

</div>
</form>
CREATE
   <h1>Criar Usuarios</h1>
<form action="{{ route('Users.store') }}" method="POST">
    @csrf
    <div>
    <h4>nome do usuario</h4>
    <input type="text" name="nome">
     <h4>email do usuario</h4>
    <input type="email" name="email">
     <h4>data de nascimento do usuario</h4>
    <input type="text" name="data_nascimento">
     <h4>jogo favorito do usuario</h4>
    <input type="text" name="jogo_favorito">
    <button type="submit">Criar</button>
    </div>
</form>
EDIT
 <h1>Lista de Usuários - Editar</h1>

    <form action ="{{ route('Users.update', $usuario->id) }}"
     method="POST">
    @csrf
    @method('PUT')
    <div class="user-card">
    Nome do Usuario
        <input type="text" name="nome" value="{{ $usuario->nome }}">
        <br>
 <input type="email" name="email" value="{{ $usuario->email }}">
        <br>
     <input type="text" name="data_nascimento" value="{{ $usuario->data_nascimento }}">
        <br>
        Jogo Favorito do Usuario
        <input type="text" name="jogo_favorito" value="{{ $usuario->jogo_favorito }}">
        
        <br>
        <button type="submit">Editar</button>
</div>
    </form>
</div>
