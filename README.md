# FLUTTER UNIT TESTING DOC

### Dependências
Primeiro mockamos as dependências que a classe testada precisa para funcionar, utilizando essa nomenclatura.

```
class CLASSEMOCKADA extends Mock implements DEPENDENCIA {}
```

Exemplo:
```
class MockAnalyticsLogger extends Mock implements AnalyticsLogger {}
```

### Função Main
Agora vamos criar a função main para podermos instanciar os objetos e criar os testes.

```
void main() {

}
```

### Instanciando Objetos
Com as dependências mockadas, agora vamos instanciar os objetos necessários para a realização dos testes.
Vamos instanciar a classe que queremos testar, a AnalyticsRouteObserver e também a classe que mockamos.

Utilizamos o setUp() para registrar as funções que devem rodar antes dos testes.

Exemplo:
```
void main() {
  late MockAnalyticsLogger mockAnalyticsLogger;
  late AnalyticsRouteObserver analyticsRouteObserver;
  
  setUp() {
    mockAnalyticsLogger = MockAnalyticsLogger();
    analyticsRouteObserver = AnalyticsRouteObserver(analytics: mockAnalyticsLogger);
  }
}

```

### Grupo de Testes
Como temos mais de um teste relacionados entre si, podemos combina-los dentro de um grupo de testes.
```
group('Funções do Analytics Router Observer', () {

});
```

### Variáveis comuns aos testes
Como iremos testar o nome das rotas, podemos criar varáveis para facilitar o processo.

```
group('Funções do Analytics Router Observer', () {
  MaterialPageRoute materialPageRouteNull = MaterialPageRoute(
    builder: (context) {
      return Container();
    },
    settings: RouteSettings(name: null),
  );

  MaterialPageRoute materialPageRouteNamed = MaterialPageRoute(
    builder: (context) {
      return Container();
    },
    settings: RouteSettings(name: 'rota'),
  );
});
```

### Criando um teste
Agora que estamos com as configurações iniciais feitas, podemos fazer o primeiro teste dentro do grupo.

Iremos testar a função didPush presente dentro da classe analyticsRouteObserver, quando a nome da rota for nulo.

#### Etapas dos Testes:
- Arrange
- Act
- Assert

#### Arrange
A etapa do **Arrange** é onde configuramos o que é necessário para o teste rodar, como inicializar variáveis, etc.
Dentro do Arrange também podemos utilizar a função when para fazermos o stub de uma classe, ou seja, definimos um comportamento previsível de retorno baseado nos parâmetros que passamos para o teste. 

É bem parecido com mockar as dependências, só que de uma forma mais simples.

#### Act
Na etapa de **Act** é onde rodamos o nosso teste, chamando a função ou método que queremos testar.

#### Assert
Na etapa de **Assert** é onde verificamos se a funcao que executamos no Act teve o resultado esperado.
Dentro do Assert podemos utilizar funções como expect, verify, verifyNoMoreInteractions.

#### Expect
A função expect é utilizado quando esperamos um resultado após a função ser executada na etapa de Act, mas também podemos colocar a função a ser executada diretamente nela, como no teste abaixo, onde temos o que é executado do lado esquerdo, e o resultado esperado no lado direito.
Pode ser utilizada no objeto da classe que estamos testando e no objeto mockado.
```
expect(() => o que é executado, o resultado esperado);
```

#### Verify
A função verify é utilizado quando queremos verificar se a função foi executada pelo menos 1 vez, por exemplo.
Também podemos usar para verificar se após a função chamada na etapa de Act é chamada, se as funções do objeto mockado são chamadas corretamente.
Só pode ser utilizada pelo objeto mockado.
```
// Para verificar se a funcao foi chamada 1 vez
verify(() => objetoMockado.funcao()).called(1);
```

#### Verify No More Interactions
A função verifyNoMoreInteractions é utilizado quando queremos verificar se não houveram mais interações no objeto mockado.
Só pode ser utilizada pelo objeto mockado.
```
verifyNoMoreInteractions(objetoMockado);
```

#### Teste
```
test('didPush se o nome da rota for nulo', () {
  // arrange -> combinar
  
  // act -> açao/agir
  final call = analyticsRouteObserver.didPush;
  
  // assert -> verificar resultado
  expect(() => call(materialPageRouteNull, materialPageRouteNull), throwsA(isA<MissingSettingsNameScreenException>()));
});

```

#### Documentação
[Documentacao Mockito](https://pub.dev/packages/mockito)
