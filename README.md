# 🏢 Sistema de Gestión de Consorcios (Database Project)

Este proyecto implementa una solución integral de base de datos relacional diseñada para la **administración y gestión de consorcios, edificios y unidades funcionales**.

Desarrollado en **Microsoft SQL Server**, el sistema abarca desde la importación masiva de datos hasta la liquidación de expensas y el manejo de seguridad de datos sensibles.

## 🚀 Funcionalidades Principales

El sistema permite modelar y administrar el ciclo de vida completo de un consorcio:

* **Gestión de Entidades:** Administración de Consorcios, Unidades Funcionales (Departamentos/Locales), Propietarios e Inquilinos.
* **Ciclo de Gastos y Expensas:** Registro de gastos mensuales, generación automática de expensas (Ordinarias y Extraordinarias) y cálculo de prorrateo por unidad.
* **Cuenta Corriente:** Registro de pagos, imputación de saldos y detección automática de morosos.
* **Importación de Datos:** Módulos dedicados para la ingesta de datos desde fuentes externas (CSV, JSON, Excel) con validación de integridad referencial.

## 🛠️ Aspectos Técnicos Destacados

Este proyecto hace uso intensivo de características avanzadas de T-SQL:

### 🔐 Seguridad y Cifrado (Encryption)

Implementación de seguridad de "Datos en Reposo" (Data at Rest). Los datos sensibles de las personas (**DNI, Email, Teléfono y CBU/CVU**) se almacenan cifrados utilizando el algoritmo **AES_256**.

* Uso de `MASTER KEY`, `CERTIFICATE` y `SYMMETRIC KEY`.
* Desencriptación al vuelo solo para usuarios/procesos autorizados.

### ⚙️ Procedimientos Almacenados (Stored Procedures)

La lógica de negocio está encapsulada en la base de datos para asegurar rendimiento y consistencia:

* **`sp_lote_expensas`:** Generación masiva de expensas del mes.
* **Importadores ETL:** Scripts robustos para importar datos (`BULK INSERT`, `OPENJSON`) con manejo de errores y limpieza de datos (trimming, validación de emails, eliminación de duplicados).

### 🛡️ Manejo de Errores y Transacciones

* Uso de bloques `TRY...CATCH` y transacciones (`BEGIN TRAN`, `COMMIT`, `ROLLBACK`) para garantizar la atomicidad de las operaciones críticas.
* Sistema de **Logging personalizado**: Los errores de importación o ejecución se registran en una tabla `ErrorLogs` para auditoría y depuración sin detener el flujo de trabajo.

## 🗄️ Modelo de Datos (Resumen)

El esquema principal incluye las siguientes tablas clave:

* **`Consorcio` / `Unidad_Funcional**`: Estructura edilicia.
* **`Persona`**: Tabla centralizada para Propietarios e Inquilinos (con columnas `VARBINARY` para datos cifrados).
* **`Gasto` / `Expensa**`: Cabeceras de liquidación mensual.
* **`Expensa_Detalle`**: Detalle de deuda por cada unidad funcional.
* **`Pago`**: Registro de cobranzas.

## 📋 Requisitos de Instalación

1. Tener instalado **SQL Server 2019** o superior.
2. Ejecutar el script de creación de objetos (`DDL`).
3. Ejecutar la creación de la infraestructura de cifrado (Keys & Certificates).
4. Ejecutar los Stored Procedures.
5. Realizar las importaciones de datos iniciales.

---

**Estado del Proyecto:** Finalizado ✅
**Tecnologías:** SQL Server, T-SQL, JSON, CSV handling.
