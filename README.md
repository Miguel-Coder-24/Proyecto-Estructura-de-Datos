# Proyecto-Estructura-de-Datos
# 📦 Sistema de Registro de Depósito y Entrega de Paquetes (C++)

> 🧩 Proyecto académico desarrollado en **C++**, que implementa estructuras de datos (arreglo dinámico, cola circular y pila) integradas en una **interfaz gráfica** para la gestión de envíos y entregas en una empresa de paquetería 24/7.

---

## 🚀 Objetivo
Desarrollar una aplicación que:
- Registre **fechas y horas** de depósito y entrega de paquetes.  
- Permita **consultas rápidas** y filtradas por fecha u hora.  
- Integre **una GUI intuitiva** con controles de calendario, formularios y tablas.  
- Aplique **estructuras de datos clásicas** para la gestión eficiente de la información.

---

## 🖥️ Interfaz Gráfica (GUI)
Implementada con **Qt** (o **wxWidgets**), diseñada para facilitar la interacción del usuario.

### 🧾 Componentes principales
- **Formulario de registro**  
  Campos: ID del paquete, cliente, fecha/hora de depósito y entrega, dirección y estado.  
  > Implementación sugerida: `QLineEdit`, `QDateTimeEdit`, `QPushButton`.

- **Calendario interactivo**  
  Visualización de movimientos diarios con `QCalendarWidget`.

- **Tabla de registros**  
  Listado de paquetes depositados o entregados por fecha, usando `QTableWidget` o `QListWidget`.

- **Filtros por hora del día**  
  Campos o botones predefinidos (“Mañana”, “Tarde”, “Noche”) para restringir los resultados mostrados.

- **Acciones CRUD**  
  Agregar, editar o eliminar registros, con confirmación y actualización inmediata.

---

## 🧠 Estructuras de Datos Utilizadas

| Estructura | Implementación | Propósito |
|-------------|----------------|------------|
| 🧮 **Arreglo dinámico (`std::vector`)** | Base de datos en memoria. | Almacena todos los paquetes, permite insertar, buscar y modificar. |
| 🔁 **Cola circular** | Implementación manual o con `std::queue`. | Guarda las últimas *N* entregas recientes en orden cronológico. |
| ↩️ **Pila (`std::stack`)** | Registro de acciones. | Implementa la función **“Deshacer” (Undo)** para revertir la última modificación. |

---

## 🧩 Diseño Modular
## 🧭 Flujo general del sistema

El siguiente diagrama muestra cómo interactúan los componentes principales del sistema de registro de paquetes en C++ con interfaz gráfica Qt.

- **MainWindow**: Ventana principal con calendario, tabla y botones.  
- **DialogoPaquete**: Formulario emergente para ingresar o editar paquetes.  
- **GestorPaquetes**: Lógica interna que maneja las estructuras de datos (`std::vector`, `std::queue`, `std::stack`).  
- **Estructuras STL**: Usadas para almacenar, registrar y deshacer operaciones.

```mermaid
flowchart TD
    %% --- Nodos principales ---
    A[👤 Usuario] -->|Abre aplicación| B[🪟 MainWindow]

    %% Interacciones principales
    B -->|Presiona "Nuevo"| C[📝 DialogoPaquete (Nuevo)]
    B -->|Presiona "Editar"| D[✏️ DialogoPaquete (Editar)]
    B -->|Presiona "Eliminar"| E[🗑️ GestorPaquetes.eliminarPaquete()]
    B -->|Presiona "Deshacer"| F[↩️ GestorPaquetes.deshacer()]
    B -->|Selecciona fecha en calendario| G[📅 Actualizar tabla del día]

    %% Desde diálogo
    C -->|Confirma datos| H[➕ agregarPaquete()]
    D -->|Confirma cambios| I[🔄 modificarPaquete()]

    %% Flujo del gestor
    H -->|Agrega al vector| J[(🗂️ Base de datos en memoria - std::vector)]
    I -->|Actualiza registro| J
    E -->|Elimina del vector| J
    F -->|Reversa última acción| J

    %% Estructuras adicionales
    J -->|Entrega completada| K[(📦 Cola circular - std::queue)]
    J -->|Registro de acción| L[(🧱 Pila de deshacer - std::stack)]

    %% Resultado visual
    J -->|Actualiza lista| M[📋 Tabla en MainWindow]
    M -->|Muestra datos actualizados| B

    %% Estilos
    classDef ui fill:#cce5ff,stroke:#004085,color:#004085,stroke-width:1px;
    classDef logic fill:#d4edda,stroke:#155724,color:#155724,stroke-width:1px;
    classDef data fill:#fff3cd,stroke:#856404,color:#856404,stroke-width:1px;

    class A,B,C,D,M ui
    class E,F,H,I,J,K,L logic
    class G data
``
---

### 🧱 1. Lógica de datos
- `struct Paquete`: contiene la información de cada envío.  
- `class GestorPaquetes`: administra las estructuras (`vector`, `queue`, `stack`) y operaciones:  
  ```cpp
  void agregarPaquete(const Paquete&);
  void eliminarPaquete(int id);
  void modificarPaquete(const Paquete&);
  void deshacer();
  std::vector<Paquete> obtenerPorFecha(const QDate& fecha);

### 🖥️ Interfaz Gráfica (GUI)

La interfaz gráfica facilita la entrada y consulta de información por parte del usuario.  
Se recomienda usar una librería multiplataforma como **Qt** (o **wxWidgets**) para crear la GUI, ya que ofrecen controles avanzados (formularios, calendarios, tablas, etc.) ideales para este sistema.

### 🧾 Componentes principales

#### 🟢 Formulario de registro de paquetes
Formulario con campos para ingresar los datos del paquete:
- **ID del paquete**: identificador único del envío.  
- **Nombre del cliente**: remitente o destinatario.  
- **Fecha y hora de depósito**: cuándo se deja el paquete para envío (`QDateTimeEdit`).  
- **Fecha y hora de entrega**: cuándo se entrega (puede ser estimada o vacía al inicio).  
- **Dirección y estado** (opcional): información adicional del envío.  

> Al presionar **“Guardar/Agregar”**, el sistema valida los campos y almacena el paquete en memoria.  
> Widgets sugeridos: `QLineEdit`, `QDateEdit`, `QTimeEdit`, `QPushButton`.

---

#### 🟣 Calendario de movimientos diarios
El calendario permite navegar entre fechas y ver los movimientos del día.  
Se puede implementar con `QCalendarWidget`, que muestra un mes completo y permite seleccionar una fecha con eventos.

- Al seleccionar una fecha, se actualiza la lista de paquetes depositados o entregados.  
- Se pueden resaltar días con movimientos mediante formatos personalizados en el calendario.  
- En wxWidgets, los equivalentes son `wxCalendarCtrl` o `wxDatePickerCtrl`.

---

#### 🟡 Listado de registros y detalle diario
Muestra todos los paquetes registrados en la fecha seleccionada (depósitos o entregas).

- Implementación sugerida: `QTableWidget` o `QListWidget`.  
- Columnas típicas: **ID**, **Cliente**, **Hora depósito**, **Hora entrega**, **Estado**.  
- Permite ordenar, filtrar y seleccionar registros individuales para editar o eliminar.

---

#### 🔵 Selección, modificación y eliminación de registros
El usuario puede seleccionar un registro y:
- **Editar:** abre el formulario con los datos precargados.  
- **Eliminar:** solicita confirmación y borra el registro.  
- Tras cada cambio, la vista se actualiza automáticamente.

> Botones sugeridos: `QPushButton("Editar")`, `QPushButton("Eliminar")`.

---

#### 🟠 Filtros por hora del día
Permiten mostrar solo los movimientos dentro de un rango horario específico (ej. 08:00–12:00).

Opciones:
- Campos “Desde” y “Hasta” (`QTimeEdit`) para definir el intervalo manualmente.  
- O botones rápidos: “Mañana”, “Tarde”, “Noche”.

Cuando se aplica un filtro, el sistema recorre los registros del día y muestra solo los que coinciden en el rango indicado.  
Esto optimiza la consulta en una empresa que opera **24 horas**.


  
