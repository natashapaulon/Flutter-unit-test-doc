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

### Main
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

```
test('didPush se o nome da rota for nulo', () {
  // arrange -> combinar
  
  // act -> açao/agir
  final call = analyticsRouteObserver.didPush;
  
  // assert -> verificar resultado
  expect(() => call(materialPageRouteNull, materialPageRouteNull), throwsA(isA<MissingSettingsNameScreenException>()));
});

```

Neste primeiro teste, 
