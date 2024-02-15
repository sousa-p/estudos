Basicamente o Dio é uma pacote para realizarmos requisições HTTP, nos permitindo ter acessos a todos os tipos de requisição: GET, PUT, DELETE, POST e PATCH. Também nos permite a configurar os headers que serão enviados, etc.

## Como usar

Requisição GET simples:

```dart
import 'package:dio/dio.dart'; // Import do módulo

final dio = Dio(); // Pegamos uma instancia do Dio

void getHttp() async {
  final response = await dio.get('https://dart.dev'); // Realizamos um requisição do tipo GET para a URL 'https://dart.dev'
  print(response); // Printaremos o resultado
}
```

Para conseguirmos enviar queries numa requisição GET pelo Dio temos duas formas:

```dart
import 'package:dio/dio.dart';

final dio = Dio();

void request() async {
  // Primeira Forma
  Response response;
  response = await dio.get('/test?id=12&name=dio');
  print(response.data.toString());


  // Segunda Forma
  response = await dio.get(
    '/test',
    queryParameters: {'id': 12, 'name': 'dio'},
  );
  print(response.data.toString());
}
```
Obs: A resultante das duas formas serão a mesma coisa.


Requisão POST:
```dart
response = await dio.post('/test', data: {'id': 12, 'name': 'dio'});
```

Para realizarmos diversas requisições de uma vez:
```dart
response = await Future.wait([dio.post('/info'), dio.get('/token')])
```

Para realizarmos download de um arquivo:
```dart
response = await dio.download(
  'https://pub.dev/',
  (await getTemporaryDirectory()).path + 'pub.html',
);
```

Para enviarmos uma requisição FormData:
```dart
final formData = FormData.fromMap({
  'name': 'dio',
  'date': DateTime.now().toIso8601String(),
});
final response = await dio.post('/info', data: formData);
```

Para fazermos Upload de um ou mais arquivos via FormData:
```dart
final formData = FormData.fromMap({
  'name': 'dio',
  'date': DateTime.now().toIso8601String(),
  'file': await MultipartFile.fromFile('./text.txt', filename: 'upload.txt'),
  'files': [
    await MultipartFile.fromFile('./text1.txt', filename: 'text1.txt'),
    await MultipartFile.fromFile('./text2.txt', filename: 'text2.txt'),
  ]
});
final response = await dio.post('/info', data: formData);
```