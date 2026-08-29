# Hefesto SII LCE

## Libros Contables Electrónicos para el SII

**Hefesto SII LCE** es una biblioteca desarrollada para facilitar la generación, estructuración y preparación de **Libros Contables Electrónicos (LCE)** destinados a los procesos del **Servicio de Impuestos Internos de Chile (SII)**.

La biblioteca proporciona una API orientada a objetos que permite al desarrollador construir los documentos LCE utilizando una estructura clara y una sintaxis fluida, evitando tener que trabajar directamente con la estructura XML.

### ¿Qué permite hacer?

| Ejemplos                | 
|-------------------------|
|[Crear envio libro diario.](# Ejemplo-crear-envio-libro-diario)|
|Crear envio libro mayor.|
|Crear envio libro balance.|

[Ver ejemplo](#ejemplo-de-comprobante)


## Ejemplo crear envio libro diario

```csharp

/// <summary>
/// Cre el envio del libro Diario
/// </summary>
static void GenerarEnvioLibroDiario()
{

    ////
    //// Recuperar los comprobantes diarios.
    var registrosDiarios = CrearRegistrosDiarios();
    var comprobantes = CrearComprobantesDiarios();

    ////
    //// crear el libro SiiLceEnvioLibros
    var bookDiario = HefestoLceElements.CrearSiiLceEnvioLibros()

        ////
        //// Configuracion del libro
        .SiiConfiguracion(cfg =>
        {
            cfg.SiiAutenticacion(auth =>
            {
                auth.Basse64 = "MIIPNgIBAzCCDvIGCSqGSIb3DQEHAaCCBf0EggX5MIIF9TCCBfEGCyqGSIb==";
                auth.Password = "1234";
            });
        })

        ////
        //// DocumentoEnvioLibros 
        .SiiDocumentoEnvioLibros(doc =>
        {
            doc.RutEnvia = "99999999-9";
            doc.RutContribuyente = "99999999-9";
            doc.SiiNotificacion(Noti =>
            {
                Noti.Tipo = SiiTipoNotificacion.Formulario3301;
                Noti.Folio = 1234;
            });

        })

        ////
        //// Agregar el libro diario.
        .SiiLibroDiario(lce =>
        {
            lce.Parcial = false;
            lce.SiiLceDiario(diario => {

                diario.ValorApertura = 1000;
                diario.SiiLceDiarioRes(res => {

                    res.SiiDocumentoDiarioRes(doc => {

                        doc.RutFirma = "99999999-9";
                        doc.SiiIdentificacion(iden => {
                            iden.RutContribuyente = "99999999-9";
                            iden.SiiPeriodoTributario(periodo =>
                            {
                                periodo.Inicial = "2026-08";
                                periodo.Final = "2026-08";
                            });
                            iden.Moneda = "CLP";
                        });

                        ////
                        //// Agregar los registros diarios.
                        doc.SiiRegistrosDiario(registrosDiarios);

                    });
                
                });

                ////
                //// Comprobantes diarios.
                diario.SiiComprobantes(comprobantes);

            });

        });

    ////
    //// Transforme el libro.
    var xml = bookDiario.ExportarLibro();
    File.WriteAllText("LibrosNormales\\LibroDiario.xml", xml, Encoding.GetEncoding("ISO-8859-1"));

}

/// <summary>
/// Crea la lista de los registros diarios del libro Diario
/// </summary>
public static List<HefRegistroDiario> CrearRegistrosDiarios()
{
    ////
    //// Lista de comprobantes diarios
    var registrosDiarios = new List<HefRegistroDiario>();

    ////
    //// Registro #1
    var registro = new HefRegistroDiario
    {
        FechaContable = "2026-08-01",
        CantidadComprobantes = 1,
        CantidadMovimientos = 5,
        SumaValorComprobante = 5179479
    };

    registrosDiarios.Add(registro);

    ////
    //// Registro #2
    registro = new HefRegistroDiario
    {
        FechaContable = "2026-08-02",
        CantidadComprobantes = 1,
        CantidadMovimientos = 3,
        SumaValorComprobante = 340658
    };

    registrosDiarios.Add(registro);
    
    ////
    //// Registro #3
    registro = new HefRegistroDiario
    {
        FechaContable = "2026-08-03",
        CantidadComprobantes = 2,
        CantidadMovimientos = 6,
        SumaValorComprobante = 509680
    };

    registrosDiarios.Add(registro);


    ////
    //// Registro #4
    registro = new HefRegistroDiario
    {
        FechaContable = "2026-08-15",
        CantidadComprobantes = 1,
        CantidadMovimientos = 2,
        SumaValorComprobante = 275680
    };

    registrosDiarios.Add(registro);

    ////
    //// Registro #5
    registro = new HefRegistroDiario
    {
        FechaContable = "2026-08-16",
        CantidadComprobantes = 1,
        CantidadMovimientos = 1,
        SumaValorComprobante = 1
    };

    registrosDiarios.Add(registro);

    ////
    //// Regrese el valor de retorno
    return registrosDiarios;

}

/// <summary>
/// Crear los comprobantes diarios del libro diario.
/// </summary>
public static List<HefComprobanteDiario> CrearComprobantesDiarios()
{
    ////
    //// Lista de comprobantes diarios
    var comprobantes = new List<HefComprobanteDiario>();

    ////
    //// Comprobante #1
    var comp = new HefComprobanteDiario()
    {

        TpoComp = "U",
        NumComp = "1",
        FechaContable = "2026-08-027",
        GlosaAnalisis = "Apertura",
        Movimientos = new List<HefMovimientoDiario>() { 
        
            new HefMovimientoDiario{ 
                
                CodigoCuenta = "1.30",
                Debe = 2900456
            },

            new HefMovimientoDiario{

                CodigoCuenta = "1.20",
                Debe = 1078567
            },

            new HefMovimientoDiario{

                CodigoCuenta = "1.40",
                Debe = 1200456
            },

            new HefMovimientoDiario{

                CodigoCuenta = "2.10",
                Haber = 560896
            },
            new HefMovimientoDiario{

                CodigoCuenta = "4.10",
                Haber = 4618583
            },
        }

    };


    comprobantes.Add(comp);


    ////
    //// Comprobante #2
    comp = new HefComprobanteDiario()
    {

        TpoComp = "U",
        NumComp = "2",
        FechaContable = "2026-08-27",
        GlosaAnalisis = "Pago de Factura",
        Movimientos = new List<HefMovimientoDiario>() {

            new HefMovimientoDiario{

                CodigoCuenta = "1.30",
                TpoDocum = "FACTURA",
                Numero = 6712,
                Haber = 340658
            },

            new HefMovimientoDiario{

                CodigoCuenta = "1.20",
                Nombre = "José Reyes",
                TpoDocum = "CHEQUE",
                Numero = 1233456,
                Debe = 300000
            },

            new HefMovimientoDiario{

                CodigoCuenta = "1.10",
                TpoDocum = "EFECTIVO",
                Debe = 40658
            },
           
        }

    };

    comprobantes.Add(comp);

    ////
    //// Comprobante #3
    comp = new HefComprobanteDiario()
    {

        TpoComp = "U",
        NumComp = "3",
        FechaContable = "2026-08-27",
        GlosaAnalisis = "Venta con Factura",
        Movimientos = new List<HefMovimientoDiario>() {

            new HefMovimientoDiario{

                CodigoCuenta = "1.30",
                Nombre = "Pedro Alzola",
                TpoDocum = "FACTURA",
                Numero = 7801,
                Debe = 234000
            },

            new HefMovimientoDiario{

                CodigoCuenta = "1.20",
                Nombre = "José Reyes",
                TpoDocum = "CHEQUE",
                Numero = 1233456,
                Debe = 300000
            },

            new HefMovimientoDiario{

                CodigoCuenta = "1.10",
                TpoDocum = "EFECTIVO",
                Debe = 40658
            },

        }

    };

    comprobantes.Add(comp);


    ////
    //// Regrese le valor de retorno.
    return comprobantes;

}


```
