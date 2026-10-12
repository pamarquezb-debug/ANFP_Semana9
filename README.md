# ANFP — Modelamiento de Bases de Datos | Semana 9

## Descripción

Proyecto académico desarrollado en pareja para la asignatura **Base de Datos** de **Duoc UC**. El trabajo aborda el modelamiento y la implementación de una base de datos relacional para un escenario de gestión de información vinculada al fútbol (ANFP).

Se diseñó un modelo lógico con especialización de entidades, se transformó al modelo relacional mediante **Oracle SQL Developer Data Modeler** y se generó el DDL para su implementación en **Oracle Cloud Autonomous Database**.

## Integrantes

- Pablo Márquez
- Segundo/a integrante: **[completar nombre]**

## Tecnologías

- Oracle SQL Developer Data Modeler 24.3
- Oracle Database (DDL generado con destino Oracle Database 21c)
- Oracle Cloud Autonomous Database
- SQL (DDL)
- GitHub

## Estructura del proyecto

```text
ANFP_Semana9/
├── README.md
├── ANFP_Semana9_DDL_corregido.sql
└── Modelo_Data_Modeler/
    └── [archivos del diseño lógico y relacional]
```

> La carpeta `Modelo_Data_Modeler` se debe completar exportando o guardando los archivos del diseño desde Data Modeler.

## Tablas implementadas

1. ASOCIACION
2. CLUB_FUTBOL
3. COMUNA
4. ESCUELA_FUTBOL
5. HISTORIAL_CLUB
6. IDIOMA
7. JUGADOR
8. JUGADOR_IDIOMA
9. NACIONALIDAD
10. PERSONAL_PLANTA
11. REGION
12. TRABAJADOR

La tabla `DBTOOLS$EXECUTION_HISTORY`, que puede aparecer al consultar el esquema desde Database Actions, es auxiliar de la herramienta y no forma parte del modelo del proyecto.

## Especialización de entidades

`TRABAJADOR` es el supertipo y contiene los atributos comunes. `JUGADOR` y `PERSONAL_PLANTA` son subtipos implementados como tablas independientes.

- `TRABAJADOR.COD_INSCRIPCION` es la clave primaria del supertipo.
- `JUGADOR.TRABAJADOR_COD_INSCRIPCION` es simultáneamente PK y FK hacia `TRABAJADOR`.
- `PERSONAL_PLANTA.TRABAJADOR_COD_INSCRIPCION` es simultáneamente PK y FK hacia `TRABAJADOR`.
- `TRABAJADOR.TIPO_TRABAJADOR` identifica el tipo de trabajador mediante una restricción `CHECK`.

> La estructura de PK/FK implementa las relaciones de especialización; el discriminador por sí solo no garantiza automáticamente todas las reglas de exclusividad entre subtipos.

## Secuencia

El script crea `SEQ_CLUB_FUTBOL` con valor inicial **703** e incremento **4**.

## Cómo ejecutar el proyecto en Oracle Cloud

1. Disponer de una instancia Oracle Autonomous Database disponible.
2. Abrir **Database Actions → SQL**.
3. Conectarse al esquema de trabajo autorizado, por ejemplo `PRY2204_S9`.
4. Verificar el esquema activo:

   ```sql
   SELECT USER FROM DUAL;
   ```

5. Abrir y ejecutar como script `ANFP_Semana9_DDL_corregido.sql` sobre un esquema que no contenga previamente las tablas del proyecto.
6. Revisar la salida para confirmar que las sentencias `CREATE` y `ALTER` finalizaron sin errores.

**Importante:** el archivo no está diseñado para ejecutarse repetidamente sobre las mismas tablas sin limpiarlas o migrarlas antes.

## Consultas de verificación

Listar las tablas:

```sql
SELECT table_name
FROM user_tables
ORDER BY table_name;
```

Comprobar claves primarias y foráneas:

```sql
SELECT constraint_type, status, COUNT(*) AS cantidad
FROM user_constraints
WHERE table_name IN (
    'ASOCIACION', 'CLUB_FUTBOL', 'COMUNA',
    'ESCUELA_FUTBOL', 'HISTORIAL_CLUB',
    'IDIOMA', 'JUGADOR', 'JUGADOR_IDIOMA',
    'NACIONALIDAD', 'PERSONAL_PLANTA',
    'REGION', 'TRABAJADOR'
)
AND constraint_type IN ('P', 'R')
GROUP BY constraint_type, status
ORDER BY constraint_type;
```

## Resultados comprobados

En la ejecución realizada en Oracle Cloud se verificó:

| Elemento | Resultado |
|---|---:|
| Tablas del proyecto | 12 |
| Claves primarias habilitadas | 12 |
| Claves foráneas habilitadas | 11 |

Estas verificaciones acreditan la creación de la estructura y el estado de las PK/FK; no equivalen a pruebas completas de carga de datos ni de todas las reglas de negocio.

## Entrega

Se recomienda incluir en el repositorio:

- Script DDL definitivo (`.sql`).
- Archivo(s) del diseño de Oracle SQL Developer Data Modeler.
- Este `README.md`.
- Evidencias de ejecución en Oracle Cloud, si las solicita la pauta de evaluación.

**Asignatura:** Base de Datos — Semana 9  
**Institución:** Duoc UC  
**Modalidad:** Trabajo en pareja
