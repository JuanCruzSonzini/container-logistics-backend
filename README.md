**Descripción del sistema**

El sistema permite gestionar solicitudes de transporte de contenedores dentro de una empresa logística. Al crear una solicitud de transporte, el usuario puede registrar un nuevo cliente en el mismo proceso o asociar
un cliente ya existente en el sistema mediante su ID. A cada solicitud se le asigna una tarifa base definida en los parámetros del sistema, que se utiliza como referencia para el cálculo inicial de los costos estimados.
Una vez creada la solicitud, indicando origen y destino, el sistema genera automáticamente tres rutas alternativas entre ambos puntos. Estas rutas pueden incluir depósitos intermedios previamente registrados en el sistema, los cuales funcionan como puntos de paso o almacenamiento temporal.
Para evaluar las rutas, el sistema calcula las distancias entre ubicaciones utilizando la fórmula de Haversine, lo que permite estimar la conveniencia de cada alternativa.
Una vez que el usuario selecciona una de las rutas generadas, el sistema calcula:

- distancia estimada
- tiempo estimado de traslado
- costo estimado del viaje

Luego la solicitud queda disponible para la asignación de un transportista y un camión a cada tramo de la ruta. Cuando ambos recursos están asignados, se puede iniciar el tramo correspondiente. El sistema no permite 
iniciar un tramo si no tiene un camión asignado. 
Al finalizar cada tramo, el transportista registra:

- tiempo real de viaje
- distancia real recorrida
- consumo real de combustible

Cuando se completa el último tramo de la ruta, el viaje se da por finalizado y la solicitud pasa a su estado final, calculándose automáticamente:

- distancia total real
- costos finales del traslado

**Gestión del sistema**

Además del flujo principal de transporte, el sistema incluye operaciones CRUD para la gestión de las entidades principales:

- transportistas
- camiones
- clientes
- contenedores
- depósitos
- parámetros del sistema

También se implementa manejo de estados para:

- solicitudes
- rutas
- tramos

La autenticación, autorización y administración de usuarios se gestiona mediante Keycloak, permitiendo definir roles y permisos específicos para cada tipo de usuario dentro del sistema.

**Cálculo de distancias con OSRM**

Para el cálculo de distancias y rutas reales entre ubicaciones se utilizó OSRM (Open Source Routing Machine).OSRM es un motor de enrutamiento de código abierto que permite calcular rutas óptimas 
utilizando datos de OpenStreetMap. A diferencia de simples cálculos geográficos, OSRM considera la red real de carreteras, generando distancias y tiempos mucho más precisos.
Para utilizarlo en el proyecto fue necesario:

1. Descargar los datos completos del mapa de Argentina de OpenStreetMap.
2. Procesar dichos datos con las herramientas de OSRM para generar el motor de enrutamiento local.
3. Ejecutar el servidor de OSRM localmente para que el backend pueda consultar las rutas y distancias mediante su API.

Debido a esto, para ejecutar correctamente el sistema es necesario descargar el mapa de Argentina y levantar el servidor de OSRM local, ya que el backend depende de este servicio para calcular las distancias
entre puntos geográficos.
