Banco de dados não relacional tipo Shared Preferences.


## Inicialização

Primeiramente temos que carregar todas as dependencias do Hive, como local aonde vamos salvar os arquivos e registrarmos os [[#adpaters]]. Para isso fiz uma classe simples com um metodo start()


```dart
import 'dart:io';
import 'package:hive_flutter/hive_flutter.dart';
import 'package:path_provider/path_provider.dart';

class HiveConfig {
  const HiveConfig();
  
  static start() async {
    Directory dir = await getApplicationDocumentsDirectory();
    await Hive.initFlutter(dir.path);
  }
}
```

Isso tudo sendo importado no main.dart

```dart
import 'package:asp/asp.dart';
import 'package:flutter/material.dart';
import 'package:hive_package/app_widget.dart';
import 'package:hive_package/core/config/hive_config.dart';
import 'package:hive_package/reducers/todo_reducer.dart';

void main() async {
  await HiveConfig.start(); // Chama para o método start, que registra o providers e etc.
  
  WidgetsFlutterBinding.ensureInitialized(); // Garante que todas as configurações seja executadas antes de renderizar os widgets
  
  TodoReducer();
  runApp(const RxRoot(child: AppWidget()));

}
```



## Adpaters


Basicamente adapters são adaptadores de nossas [[#classes]], para que o Hive consiga salvar adequadamente nossos dados, pois o Hive consegue apenas manipular tipos primitivos, ou seja, descrevemos os equivalentes para cada atributo de nossa classe.

```
import 'package:hive/hive.dart';
import 'package:hive_package/entities/todo_entity.dart';

class TodoHiveAdapter extends TypeAdapter<TodoEntity> {
  @override
  
  final typeId = 1; // Endereço da classe no hive vai de 0 a 259
  @override

  TodoEntity read(BinaryReader reader) {
  // Descreve como vai ser a leitura na hora de lermos essa classe, ou seja, ela recebe dois dados do tipo string
  
    return TodoEntity(reader.readString(), reader.readString());
  }

  @override

  void write(BinaryWriter writer, TodoEntity obj) {
  // Aqui ele descreve os equivalentes para criarmos o objeto, descrevendo que tipo de dado sera escrito em cada atributo
  
    writer.writeString(obj.title);
    writer.writeString(obj.dateToFinish);
    writer.writeString(obj.creationDate);
    writer.writeBool(obj.finished);

  }

}
```


## Classes

Para criarmos uma classe que será posteriormente salva no Hive, devemos fazer alguns tipos de anotações para descrevermos de que forma será salvo dentro do DB.

```
import 'package:hive_flutter/hive_flutter.dart'; 

part 'todo_entity.g.dart'; // Arquivo gerado pelo build runner

@HiveType(typeId: 1) // Endereço de locação da classe no Hive
class TodoEntity {
  @HiveField(0, defaultValue: 0) // Endereço de locação do atributo no Hive e o valor caso seja nulo, ou seja, 0
  final String title;

  @HiveField(1)
  final String dateToFinish;

  @HiveField(2, defaultValue: '')
  final String creationDate = DateTime.now().toString();

  @HiveField(3, defaultValue: false)
  bool finished = false;
  
  TodoEntity(this.title, this.dateToFinish);

}
```

Observação: Caso já tenhamos dados pré-salvos, e mudemos a ordem do endereço de locação do objeto, dará erro.

## Box

Trocando por miúdos a box é a nossa tabela, devemos abrir a box para termos acesso aos dados dentro da mesma.

Observação: Caso tentemos abrir uma box, já aberta, dará erro.


```
class TodoReducer extends Reducer {
  late final Box box;

  TodoReducer() {
    _openBox();
	...
  }

  

  void _openBox() async {
    Hive.registerAdapter(TodoHiveAdapter()); // Neste caso eu estou registrando o adapter aqui mesmo
    
    box = await Hive.openBox<TodoEntity>('todo'); // Abrimos a caixa e passamos o generic dele
    
    ...

  }
...
```


## Operações CRUD

Aqui mostrarei um exemplo de código para saber como realizar operações CRUD com o Hive.

```
void _saveTodo() async {
    final TodoEntity todo = todoState.value!; // Temos que ter um objeto esperado pelo box, neste caso um TodoEntity
    
    await box.add(todo); // E Salvamos ele com o método box.add, note que é assincrono
  }

  void _changeStatusTodo() async {
    final index = selectedTodoState.value;
    final todo = allTodosState.value![index!];

    todo.finished = !todo.finished;
    await box.put(index, todo);
    _getAllTodos();
  }

  void _deleteTodo() async {
    final index = selectedTodoState.value;
    await box.deleteAt(index!);
    _getAllTodos();
  }


  void _getAllTodos() async {
    allTodosState.setValue(box.values.toList() as List<TodoEntity>);
  }
```



[Documentação Oficial](https://docs.hivedb.dev)