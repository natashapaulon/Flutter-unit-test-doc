# Teste Analytics Completo

```
import 'package:flutter/material.dart';
import 'package:mobile_app/features_core/analytics/analytics_logger.dart';
import 'package:mobile_app/features_core/analytics/analytics_route_observer.dart';
import 'package:mobile_app/features_core/analytics/analytics_router_exceptions.dart';
import 'package:mocktail/mocktail.dart';
import 'package:test/expect.dart';
import 'package:test/scaffolding.dart';

class MockAnalyticsLogger extends Mock implements AnalyticsLogger {}

class MockRoute extends Mock implements Route {}

void main() {
  late AnalyticsRouteObserver analyticsRouteObserver;
  late MockAnalyticsLogger mockAnalyticsLogger;

  setUp(() {
    mockAnalyticsLogger = MockAnalyticsLogger();
    analyticsRouteObserver = AnalyticsRouteObserver(analytics: mockAnalyticsLogger);
  });

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

    test('didPush se o nome da rota for nulo', () {
      final call = analyticsRouteObserver.didPush;

      expect(() => call(materialPageRouteNull, materialPageRouteNull), throwsA(isA<MissingSettingsNameScreenException>()));
    });

    test('didPush se o nome da rota não for nulo', () {
      analyticsRouteObserver.didPush(materialPageRouteNamed, materialPageRouteNamed);

      verify(() => analyticsRouteObserver.didPush(materialPageRouteNamed, materialPageRouteNamed)).called(1);
    });

    test('didReplace se o nome da rota for nulo', () {
      final call = analyticsRouteObserver.didReplace;

      expect(
          () => call(newRoute: materialPageRouteNull, oldRoute: materialPageRouteNull), throwsA(isA<MissingSettingsNameScreenException>()));
    });

    test('didReplace se o nome da rota não for nulo', () {
      analyticsRouteObserver.didReplace(newRoute: materialPageRouteNamed, oldRoute: materialPageRouteNamed);

      verify(() => analyticsRouteObserver.didReplace(newRoute: materialPageRouteNamed, oldRoute: materialPageRouteNamed)).called(1);
    });

    test('didPop se o nome da rota for nulo', () {
      final call = analyticsRouteObserver.didPop;

      expect(() => call(materialPageRouteNull, materialPageRouteNull), throwsA(isA<MissingSettingsNameScreenException>()));
    });

    test('didPop se o nome da rota não for nulo', () {
      analyticsRouteObserver.didPop(materialPageRouteNamed, materialPageRouteNamed);

      verify(() => analyticsRouteObserver.didPop(materialPageRouteNamed, materialPageRouteNamed)).called(1);
    });
  });
}
```
