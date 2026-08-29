# Hefesto Lce

# Hefesto SII LCE

## Libros Contables Electrónicos para el SII

**Hefesto SII LCE** es una biblioteca desarrollada para facilitar la generación, estructuración y preparación de **Libros Contables Electrónicos (LCE)** destinados a los procesos del **Servicio de Impuestos Internos de Chile (SII)**.

La biblioteca proporciona una API orientada a objetos que permite al desarrollador construir los documentos LCE utilizando una estructura clara y una sintaxis fluida, evitando tener que trabajar directamente con la estructura XML.

### ¿Qué permite hacer?

Hefesto SII LCE permite construir los distintos componentes que forman parte de un Libro Contable Electrónico, incluyendo:

* **Libro Diario**
* **Libro Mayor**
* **Comprobantes contables**
* **Movimientos contables**
* **Registros diarios**
* **Cierres y resúmenes**
* **Notificaciones**
* **Configuración y autenticación para procesos SII**
* Generación del documento XML correspondiente

La biblioteca está diseñada para que el desarrollador pueda trabajar con sus propios datos, provenientes de bases de datos, sistemas contables, ERP u otras fuentes, y posteriormente incorporarlos al modelo requerido por el SII.

### Una forma sencilla de construir un LCE

El objetivo de Hefesto es que el desarrollador pueda concentrarse en **los datos y la lógica de su aplicación**, mientras la biblioteca se encarga de representar dichos datos en la estructura correspondiente.

Por ejemplo:

```csharp
var libro = HefestoLceElements
    .CrearSiiLceEnvioLibros()

    .SIIConfiguracion(cfg =>
    {
        cfg.SIIAutenticacion(auth =>
        {
            auth.Basse64 = "...";
            auth.Password = "...";
        });
    })

    .SiiDocumentoEnvioLibros(doc =>
    {
        doc.RutEnvia = "1-9";
        doc.RutContribuyente = "2-8";
    });
```

De esta manera, la aplicación puede construir progresivamente el documento utilizando métodos que representan los elementos del estándar LCE.

### Diseñado para trabajar con datos reales

Hefesto SII LCE no obliga al usuario a construir manualmente cada elemento del documento.

Los datos pueden provenir directamente de las estructuras que ya utiliza la aplicación:

```text
Base de datos
     │
     ▼
Sistema contable / ERP
     │
     ▼
Objetos de negocio
     │
     ▼
Hefesto SII LCE
     │
     ▼
Documento LCE
     │
     ▼
XML
     │
     ▼
Proceso SII
```

Esto permite que una aplicación pueda generar desde unos pocos comprobantes hasta **miles de registros y movimientos**, utilizando las propias colecciones de datos del sistema.

### El propósito de Hefesto

La idea detrás de Hefesto SII LCE es sencilla:

> **Que el desarrollador trabaje con objetos y datos de su aplicación, y no tenga que pelearse directamente con el XML.**

El XML es el resultado final del proceso, no el lugar donde debería comenzar el trabajo.

Hefesto busca entregar una capa de abstracción entre la aplicación y la estructura técnica requerida para los procesos de Libros Contables Electrónicos del SII, manteniendo el modelo suficientemente flexible para adaptarse a las necesidades de cada sistema.

---

## Documentación

En esta documentación encontrarás ejemplos de utilización, descripción de las clases, métodos disponibles y formas recomendadas de construir cada uno de los componentes de un Libro Contable Electrónico.

**Bienvenido a Hefesto SII LCE.** 🔥
