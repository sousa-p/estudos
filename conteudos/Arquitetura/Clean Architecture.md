O Clean ele é uma ideia, para que consigamos fazer implementações em nosso projeto de maneira limpa e desacoplada, ou seja, não possuindo dependências diretas de packages, classes, funções e sim de interfaces, classes abstratas, classes seladas e etc. Com isto, possuímos uma [[Inversão de Dependências]], na qual, independente da forma que iremos realizar, somos respaldados pelos contratos que garantem e descrevem as estruturas presentes em nosso código.

Vale-se ressaltar, que não existe uma implementação certa ou errada do Clean, estruturas, nomes de pastas, podem variar conforme a linguagem e projeto, entretanto, o Clean é um conjunto de boas práticas para um código desacoplado. Tendo isto em vista, irei dar uma ênfase, a utilização do Clean, no Dart.

![[clean_dart.png]]
Essa seria a representação visual de uma implementação "limpa". Onde, cada camada são independentes, e mutualmente se garantem, sem existir uma dependência propriamente dito. Lembre-se que toda estrutura dentro do seu código, deve apenas, e exclusivamente realizar apenas o que se diz ser, exemplificando, se uma estrutura é responsável por gerenciar o estado da sua aplicação, a mesma não poderá, e nem deve, em por exemplo, fazer resgate de dados no bd, isso vale para **TUDO** em seu código.


## Domain

O Domain é a estrutura responsável por ditar, as formas que as coisas terão em nosso código, logo, nossas entidades serão descritas ai, nossas classes abstratas serão descritas ai,  nossa implementação de use cases, como também o próprio.

- **Entities**: São  a representação de um objeto em nosso sistema, a garantia, de que dados uma entidade possui dentro de nosso sistema.
 
- **Use Cases**: Nossos use cases, basicamente são cada ação dentro de nosso sistema,  dentro de um arquivo de use case, teremos uma classe abstrata que garante o comportamento de nosso use case, e a implementação do mesmo, que geralmente apenas apresentará um método call, vale ressaltar, que sim, pode ter mais métodos, entretanto, não podemos ferir o principio de responsabilidade única.

-  Para mais informações: [[Classes abstratas]], [[Classe selada]]

## Infrastructure (Infra)

O Infra será quem vai aplicar/implementar as interfaces do [[#Domain]], como também, tem o papel de adaptar dados externos, por meio de estruturas chamadas Adapters (descritivo não?), nele as estruturas que antes foram descritas no domain por meio das estrutras já comentadas irão tomar forma e serão utilizadas/seguidas.

- **Repository**: Ele vai ser a estrutura responsável por garantir que o que esta chegando no data source, é o que espera-se no use case, e vice-versa. [Mais informações](https://paulallies.medium.com/clean-architecture-repositories-a3360184dd66)

## External

Aqui é onde iremos implementar os acessos externos que nossa aplicação irá possuir, acessos de APIs, packages, ou acessos muito especificos dentro de nossa aplicação.

"*No Flutter por exemplo, para cache local usamos o SharedPreferences, mas talvez em alguma estágio do projeto a implementação do SharedPreferences não seja mais suficiente para a aplicação e deve ser substituída por outro package como Hive, nesse ponto a única coisa que precisamos fazer é criar uma nova classe, implementando o Contrato esperado pela camada mais alta (que seria a Infra) e implementarmos a lógica usando o Hive.*

*Um outro exemplo prático seria pensar em um login com Firebase Auth, porém outro produto deseja utilizar um outro provider de autenticação. Bastaria apenas implementar um datasource baseado no outro provider e “Inverter a Dependência” substituindo a implementação do Firebase pela nova quando for necessário.*

*Os Datasources devem se preocupar apenas em “descobrir” os dados externos e enviar para a camada de Infra para serem tratados.*" - [Fonte](https://github.com/Flutterando/Clean-Dart)

## Presenter

Basicamente essa é a camada visual de nosso sistema, ela é onde vamos ter os inputs e outputs, lá é onde teremos nossos componentes, gerenciamento de estado.


## Considerações finais

- Sempre dependa das classes abstratas e interfaces nunca de implementações propriamente dito,
- Tente sempre utilizar injeção de dependência.
- Não tente driblar as regras pré-estabelecidas pelo.

## Links

[Livro Arquitetura Limpa](https://github.com/free-educa/books/blob/main/books/Arquitetura%20Limpa%20-%20O%20Guia%20do%20Artes%C3%A3o%20para%20Estrutura%20e%20Design%20de%20Software%20(Robert%20C.%20Martin)%20(z-lib.org).pdf)
[Clean Dart Flutterando](https://github.com/Flutterando/Clean-Dart)
[Resumo do Livro Arquitetura Limpa](https://deividwillyan.medium.com/desvendando-a-arquitetura-limpa-de-uncle-bob-3e60d9aa9cce)
[Playlist de vídeos sobre Arquitetura Limpa](https://youtube.com/playlist?list=PLlBnICoI-g-d-v_fWlkZX2HRgHHPnJx9s&si=4js2KrDmkGT5sJBv)

![[Arquitetura Limpa - O Guia do Artesão para Estrutura e Design de Software (Robert C. Martin) (z-lib.org).pdf]]