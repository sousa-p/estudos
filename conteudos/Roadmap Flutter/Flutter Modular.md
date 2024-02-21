
Basicamente ele transforma o Flutter em algo bem parecido com o Angular, ou seja, temos módulos, independentes entre si, que apenas chamam as dependências, neste caso as Binds, de maneira unitária.

```dart

class AppModule extends Module {  
	@override  
	void binds(i) {  
		i.add(XPTOEmail.new);  
		i.add<EmailService>(XPTOEmailService.new);  
		i.addSingleton(Client.new);  
		  
		// Register with Key  
		i.addSingleton(Client.new, key: 'OtherClient');  
	}  
  
...  
}

```

Nisto, ele também modulariza as rotas dentro do de cada modulo, muito parecido com o angular e suas sub rotas.

```dart
class AppModule extends Module {  
	@override  
	void binds(i) {}  
	  
	@override  
	void routes(r) {  
	r.child('/', child: (context) => HomePage()),  
	r.child('/second', child: (context) => SecondPage()),  
	}  
}
```