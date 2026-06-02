# Juego de Exploración — Sistema de Rutas y Jugadores

> Obligatorio de Algoritmos y Estructura de Datos 2 · Universidad · Java · 2023

Sistema que modela una red de ciudades conectadas por caminos, donde jugadores exploran el mapa respondiendo preguntas. Implementa estructuras de datos y algoritmos desde cero en Java puro, sin librerías externas.

---

## Algoritmos implementados

| Algoritmo | Descripción |
|---|---|
| **Dijkstra (km)** | Camino mínimo entre dos ciudades por kilómetros; descarta rutas en mal estado |
| **Dijkstra (monedas)** | Camino mínimo por costo económico |
| **BFS por niveles** | Ciudades alcanzables desde un origen en ≤ N saltos |
| **Inserción/búsqueda ABB** | Árbol Binario de Búsqueda para gestión de jugadores |

## Estructuras de datos propias

```
ABB<T>            — Árbol Binario de Búsqueda genérico con traversal por Visitor
Grafo             — Matriz de adyacencia N×N con aristas multi-peso (km, costo, tiempo, estado)
ListaGenerica<T>  — Lista enlazada genérica (cola en BFS, serialización de resultados)
Tupla<A, B>       — Par genérico para retornar (camino reconstruido, costo total)
```

## Arquitectura

El sistema sigue una separación estricta en capas:

```
interfaz/     → Sistema.java (contrato público), Retorno, TipoJugador, EstadoCamino
sistema/      → ImplementacionSistema (lógica de negocio)
dominio/      → Jugador, CentroUrbano, Cedula + estructuras propias
excepciones/  → 12 excepciones tipadas (una por caso de error)
```

El patrón **Visitor** desacopla los traversals de las estructuras: 6 visitors distintos para serializar, filtrar, invertir orden y agregar elementos.

## Mecánica de exploración

Al explorar una ciudad el jugador responde un quiz. El puntaje acumula **bonificaciones por rachas**:

- 3 correctas seguidas → +3 pts
- 4 seguidas → +5 pts
- 5 o más → +8 pts

Si el total supera el mínimo del nivel, el jugador pasa al siguiente centro.

## Tests

15 suites JUnit 5 con cobertura de happy path y todos los códigos de error (ERROR_1 a ERROR_5):

```
InicializarSistemaTest       RegistrarJugadorTest        BuscarJugadorTest
RegistrarCentroUrbanoTest    RegistrarCaminoTest          ActualizarCaminoTest
ExplorarCentroUrbanoTest     ListarCedulaAscendenteTest  ListarCedulaDescendenteTest
ListarJugadoresPorTipoTest   FiltrarJugadoresTest         ListadoCentrosCantDeSaltosTest
ViajeCostoMinimoKilometrosTest  ViajeCostoMinimoMonedasTest  TestSistemaAFuturo
```

## Stack

- **Java** (sin frameworks de aplicación)
- **JUnit Jupiter 5.8.1** para tests
- **GraphViz** (integración opcional para visualizar el grafo)
