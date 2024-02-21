
Basicamente é o conceito que diz, que os módulos dentro de um sistema devem sempre depender de abstrações e não de implementações. Do mesmo modo, que as abstrações não podem ser dependentes de um módulo ou componente especifico.

## Exemplo

Ao criarmos um módulo que lida com banco de dados, poderíamos escolher por exemplo o MySQL, onde todo o seu sistema, irá depender desta implementação para manipular este banco, entretanto, caso mudemos de banco de dados para um SQLServer, deveríamos mudar todo o nosso código pois este esta totalmente acoplado ao MySQL. Mas caso utilizemos abstrações por meio do POO com [[Classes abstratas]] e [[Classe selada]], poderíamos ter um conjunto abstrato de ações onde nossa implementação a segue, deste modo apenas mudaríamos a implementação que segue nossa abstração, e não todo o sistema.

Isto é possível, pois a abstração nos garante os métodos e atributos que nossas classes sejam de MySQL, PostgreSQL, SQLServer, terão.

