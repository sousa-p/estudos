Basicamente temos um arquivos de estados,no qual temos as Actions e o estados em nosso sistema.

- **Actions**: Ações que serão emitidas e capturadas em nosso reducer, não podem assumir valor diferente de RxVoid.

- **Estados**: Armazenam dados, que serão escutados em nossas páginas, que geralmente são alterados pelo nosso reducer, quando escutamos uma Action.

```dart
import 'package:rx_notifier/rx_notifier.dart';

// Atom que sera modificado pelas actions
final counterState = RxNotifier(0);

// Actions que serão chamadas
final decreamentAction = RxNotifier.action();
final incrementAction = RxNotifier.action();

```


Com isto temos uma classe chamada reducer, sendo ele o cara responsável por escutar as actions e possivelmente alterar o Atom de um estado.

```dart
import 'package:rx_notifier/rx_notifier.dart';
import 'package:asp/state.dart';


class CounterReducer extends RxReducer {
  CounterReducer() {
    on(() => [incrementAction.value], increment);
    on(() => [decreamentAction.value], decrement);
  }

  void increment() {
    counterState.value++;
  }

  void decrement() {
    counterState.value--;
  }

}
```


Claro devemos instanciar o reducer em algum escopo para que nosso widgets tenham acesso.

```dart
import 'package:asp/pages/home_page.dart';
import 'package:asp/reducers/couter_reducer.dart';
import 'package:flutter/material.dart';
import 'package:rx_notifier/rx_notifier.dart';


class App extends StatelessWidget {
  const App({super.key});
  @override
  
  Widget build(BuildContext context) {
    CounterReducer();
    return const RxRoot(
        child: MaterialApp(
      home: HomePage(),
    ));
  }
}
```


Desta forma, nossa View não irá saber de nada que ocorrerá quando chama uma action, ela apenas o chama, como também, apenas rebuildar, o Atom alterado, assim ganhamos performance e comodidade em nosso código.

```dart
// Define o átomo que iremos utilizar
final counter = context.select(() => counterState.value);

// Chama uma action
FloatingActionButton(
	onPressed: incrementAction.call,
	tooltip: 'Increment',
	child: const Icon(Icons.add),
),
```


[Documentação Oficial](https://pub.dev/documentation/rx_notifier/latest/)