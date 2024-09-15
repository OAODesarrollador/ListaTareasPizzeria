
# Documentación - Lista de Tareas (ToDo List) para una Pizzería

## Integrantes del Grupo:

- Leonel Giralde
- Oscar Ortiz
- Casimiro Daniel
- Marcelo Giglio

## Concepto General

El objetivo de este proyecto es desarrollar una lista de tareas (ToDo list) personalizada para una pizzería, que permita a los empleados organizar y gestionar tareas operativas diarias, como preparar pedidos, atender clientes, limpiar áreas, etc. La aplicación contará con una interfaz web simple, accesible desde diferentes dispositivos, que permita a los usuarios agregar, modificar, eliminar y marcar como completadas las tareas de manera eficiente.

El sistema también contará con una base de datos relacional que almacenará las tareas persistentes y permitirá el manejo adecuado de los datos.

### Casos de Uso

1. **Agregar nueva tarea**: El usuario puede escribir una tarea en un campo de texto y añadirla a la lista.
2. **Marcar tarea como completada**: El usuario puede marcar una tarea como completada, lo que permitirá diferenciar las tareas realizadas de las pendientes.
3. **Eliminar tarea**: El usuario puede eliminar tareas que ya no son necesarias.
4. **Visualizar lista de tareas**: El usuario puede visualizar todas las tareas en curso y las completadas.
5. **Editar tarea**: El usuario puede modificar una tarea existente en caso de que se haya cometido un error o haya cambios en la tarea asignada.
6. **Sincronización**: Las tareas deben estar sincronizadas con la base de datos, de manera que, si un usuario actualiza la lista, todos los usuarios verán los cambios en tiempo real.

### Estructura de la Base de Datos

#### Entidades Principales

##### 1. **Tareas**

Esta entidad almacena las tareas que deben ser gestionadas en la aplicación.

| Campo           | Tipo de Dato       | Descripción                                      |
|-----------------|--------------------|--------------------------------------------------|
| `id`            | INT (PK)           | Identificador único de la tarea                  |
| `tarea`         | VARCHAR(255)       | Descripción o texto de la tarea                  |
| `estadoTarea`   | BOOLEAN            | Estado de la tarea (true = completada, false = pendiente) |
| `creadoEl`      | TIMESTAMP          | Fecha y hora en la que se creó la tarea          |
| `actualizadoEl` | TIMESTAMP          | Fecha y hora en la que se actualizó la tarea     |

##### 2. **Usuarios**

Esta entidad almacena información de los usuarios que interactúan con la lista de tareas. Si es necesario diferenciar entre diferentes tipos de usuarios (por ejemplo, administradores y empleados), podemos agregar más atributos a esta tabla.

| Campo           | Tipo de Dato       | Descripción                                      |
|-----------------|--------------------|--------------------------------------------------|
| `id`            | INT (PK)           | Identificador único del usuario                  |
| `nombreUsuario` | VARCHAR(100)       | Nombre de usuario del sistema                    |
| `contrasenia`   | VARCHAR(10)        | Contraseña para autenticación                    |
| `rol`           | VARCHAR(20)        | Rol del usuario (por ejemplo, "admin", "staff")  |
| `fechaCreacion` | TIMESTAMP          | Fecha de creación del usuario                    |

##### 3. **Relación entre Tareas y Usuarios (tareaAsignada)**

Esta entidad es necesaria para asignar tareas a usuarios específicos en caso de que haya múltiples empleados trabajando con la lista de tareas.

| Campo           | Tipo de Dato       | Descripción                                      |
|-----------------|--------------------|--------------------------------------------------|
| `tarea_id`      | INT (FK)           | Referencia al ID de la tarea                     |
| `usuario_id`    | INT (FK)           | Referencia al ID del usuario                     |
| `fechaAsignada` | TIMESTAMP          | Fecha en la que se asignó la tarea               |

#### Relación entre Tablas

1. **Claves Primarias**:
   - `id` es la clave primaria en las tablas `Tarea` y `Usuario`.
   - `tarea_id` y `usuario_id` son claves foráneas en la tabla `tareaAsignada`, que crean la relación entre las tareas y los usuarios.

2. **Claves Foráneas**:
   - `tarea_id` en la tabla `tareaAsignada` es una clave foránea que referencia a `Tareas(id)`.
   - `usuario_id` en la tabla `Task_Assignments` es una clave foránea que referencia a `Usuarios(id)`.

### Tipos de Variables y Atributos

1. **`id`**:
   - Tipo: `INT`
   - Descripción: Identificador único para cada tarea y usuario. Sirve como clave primaria.

2. **`texto`**:
   - Tipo: `VARCHAR(255)`
   - Descripción: Texto descriptivo de la tarea.

3. **`estadoTarea`**:
   - Tipo: `BOOLEAN`
   - Descripción: Indica si la tarea está completada (true) o pendiente (false).

4. **`creadoEl`**:
   - Tipo: `TIMESTAMP`
   - Descripción: Fecha y hora en que se creó la tarea.

5. **`actualizadoEl`**:
   - Tipo: `TIMESTAMP`
   - Descripción: Fecha y hora de la última modificación de la tarea.

6. **`nombreUsuario`**:
   - Tipo: `VARCHAR(100)`
   - Descripción: Nombre de usuario para identificar a los empleados o administradores.

7. **`contrasenia`**:
   - Tipo: `VARCHAR(255)`
   - Descripción: Contraseña encriptada para la autenticación del usuario.

8. **`rol`**:
   - Tipo: `VARCHAR(50)`
   - Descripción: Rol del usuario dentro de la pizzería (ej. administrador, personal).

### Ejemplos de Consultas SQL

1. **Crear la tabla de tareas**:

```sql
CREATE TABLE Tareas (
  id INT AUTO_INCREMENT PRIMARY KEY,
  texto VARCHAR(255) NOT NULL,
  estado BOOLEAN DEFAULT FALSE,
  creadoEl TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  actualizadoEl TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```

2.**Crear la tabla de usuarios**:

```sql
CREATE TABLE Usuarios (
  id INT AUTO_INCREMENT PRIMARY KEY,
  nombreUsuario VARCHAR(100) NOT NULL,
  contrasenia VARCHAR(10) NOT NULL,
  rol VARCHAR(20) NOT NULL,
  fechaCreacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

3.**Crear la tabla de asignaciones de tareas**:

```sql
CREATE TABLE tareaAsignada (
  tarea_id INT,
  usuario_id INT,
  fechaAsignada TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (task_id, user_id),
  FOREIGN KEY (task_id) REFERENCES tasks(id),
  FOREIGN KEY (user_id) REFERENCES users(id)
);
```

### Conclusión

Como motor de base de datos consideramos usar MySQL, ya que es el sistema de gestión de base de datos más popular.
Esta lista de tareas para una pizzería está diseñada para ser fácil de usar y extensible, con una estructura de base de datos bien definida que permite gestionar las tareas y usuarios de manera eficiente. Además, es posible expandir el sistema para incluir más funcionalidades como la asignación de tareas a usuarios específicos, generar reportes de tareas completadas, y mucho más.
