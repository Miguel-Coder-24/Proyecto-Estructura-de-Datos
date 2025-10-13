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

### 🧱 1. Lógica de datos
- `struct Paquete`: contiene la información de cada envío.  
- `class GestorPaquetes`: administra las estructuras (`vector`, `queue`, `stack`) y operaciones:  
  ```cpp
  void agregarPaquete(const Paquete&);
  void eliminarPaquete(int id);
  void modificarPaquete(const Paquete&);
  void deshacer();
  std::vector<Paquete> obtenerPorFecha(const QDate& fecha);
