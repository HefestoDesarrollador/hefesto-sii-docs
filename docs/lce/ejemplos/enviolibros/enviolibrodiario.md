## Ejemplo envio libro diario


```csharp

/// <summary>
/// Cre el envio del libro Diario
/// </summary>
static void GenerarEnvioLibroDiario()
{

    ////
    //// Recuperar los registros y comprobantes diarios
    //// para completar el libro.
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


```

```csharp

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

```

```csharp

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

#Resultado de la operación.


```XML

<?xml version="1.0" encoding="iso-8859-1"?>
<LceEnvioLibros version="1.0">
<!-- Autor: Hefesto -->
  <DocumentoEnvioLibros ID="554698a6-3769-43c6-90ae-222d48f2f9ea">
    <RutEnvia>99999999-9</RutEnvia>
    <RutContribuyente>99999999-9</RutContribuyente>
    <Notificacion>
      <Tipo>1</Tipo>
      <Folio>1234</Folio>
    </Notificacion>
    <TmstFirmaEnv>2026-08-28T20:51:46</TmstFirmaEnv>
  </DocumentoEnvioLibros>
  <LCE>
    <LceDiario version="1.0">
      <LceDiarioRes version="1.0">
        <DocumentoDiarioRes ID="f7730a03-78f5-400d-977f-8e5b5ebf6395">
          <Identificacion>
            <RutContribuyente>99999999-9</RutContribuyente>
            <PeriodoTributario>
              <Inicial>2026-08</Inicial>
              <Final>2026-08</Final>
            </PeriodoTributario>
            <Moneda>CLP</Moneda>
          </Identificacion>
          <RegistroDiario>
            <FechaContable>2026-08-01</FechaContable>
            <CantidadComprobantes>1</CantidadComprobantes>
            <CantidadMovimientos>5</CantidadMovimientos>
            <SumaValorComprobante>5179479</SumaValorComprobante>
          </RegistroDiario>
          <RegistroDiario>
            <FechaContable>2026-08-02</FechaContable>
            <CantidadComprobantes>1</CantidadComprobantes>
            <CantidadMovimientos>3</CantidadMovimientos>
            <SumaValorComprobante>340658</SumaValorComprobante>
          </RegistroDiario>
          <RegistroDiario>
            <FechaContable>2026-08-03</FechaContable>
            <CantidadComprobantes>2</CantidadComprobantes>
            <CantidadMovimientos>6</CantidadMovimientos>
            <SumaValorComprobante>509680</SumaValorComprobante>
          </RegistroDiario>
          <RegistroDiario>
            <FechaContable>2026-08-15</FechaContable>
            <CantidadComprobantes>1</CantidadComprobantes>
            <CantidadMovimientos>2</CantidadMovimientos>
            <SumaValorComprobante>275680</SumaValorComprobante>
          </RegistroDiario>
          <RegistroDiario>
            <FechaContable>2026-08-16</FechaContable>
            <CantidadComprobantes>1</CantidadComprobantes>
            <CantidadMovimientos>1</CantidadMovimientos>
            <SumaValorComprobante>1</SumaValorComprobante>
          </RegistroDiario>
          <Cierre>
            <CantidadComprobantes>6</CantidadComprobantes>
            <CantidadMovimientos>17</CantidadMovimientos>
            <SumaValorComprobante>6305498</SumaValorComprobante>
            <ValorAcumulado>6305498</ValorAcumulado>
          </Cierre>
          <RutFirma>99999999-9</RutFirma>
          <TmstFirma>2026-08-28T20:51:46</TmstFirma>
        </DocumentoDiarioRes>
      </LceDiarioRes>
      <Comprobante>
        <TpoComp>U</TpoComp>
        <NumComp>1</NumComp>
        <FechaContable>2026-08-027</FechaContable>
        <GlosaAnalisis>Apertura</GlosaAnalisis>
        <Movimientos>
          <CodigoCuenta>1.30</CodigoCuenta>
          <Debe>2900456</Debe>
        </Movimientos>
        <Movimientos>
          <CodigoCuenta>1.20</CodigoCuenta>
          <Debe>1078567</Debe>
        </Movimientos>
        <Movimientos>
          <CodigoCuenta>1.40</CodigoCuenta>
          <Debe>1200456</Debe>
        </Movimientos>
        <Movimientos>
          <CodigoCuenta>2.10</CodigoCuenta>
          <Haber>560896</Haber>
        </Movimientos>
        <Movimientos>
          <CodigoCuenta>4.10</CodigoCuenta>
          <Haber>4618583</Haber>
        </Movimientos>
        <ValorComprobante>0</ValorComprobante>
      </Comprobante>
      <Comprobante>
        <TpoComp>U</TpoComp>
        <NumComp>2</NumComp>
        <FechaContable>2026-08-27</FechaContable>
        <GlosaAnalisis>Pago de Factura</GlosaAnalisis>
        <Movimientos>
          <CodigoCuenta>1.30</CodigoCuenta>
          <TpoDocum>FACTURA</TpoDocum>
          <Numero>6712</Numero>
          <Haber>340658</Haber>
        </Movimientos>
        <Movimientos>
          <CodigoCuenta>1.20</CodigoCuenta>
          <Nombre>José Reyes</Nombre>
          <TpoDocum>CHEQUE</TpoDocum>
          <Numero>1233456</Numero>
          <Debe>300000</Debe>
        </Movimientos>
        <Movimientos>
          <CodigoCuenta>1.10</CodigoCuenta>
          <TpoDocum>EFECTIVO</TpoDocum>
          <Debe>40658</Debe>
        </Movimientos>
        <ValorComprobante>0</ValorComprobante>
      </Comprobante>
      <Comprobante>
        <TpoComp>U</TpoComp>
        <NumComp>3</NumComp>
        <FechaContable>2026-08-27</FechaContable>
        <GlosaAnalisis>Venta con Factura</GlosaAnalisis>
        <Movimientos>
          <CodigoCuenta>1.30</CodigoCuenta>
          <Nombre>Pedro Alzola</Nombre>
          <TpoDocum>FACTURA</TpoDocum>
          <Numero>7801</Numero>
          <Debe>234000</Debe>
        </Movimientos>
        <Movimientos>
          <CodigoCuenta>1.20</CodigoCuenta>
          <Nombre>José Reyes</Nombre>
          <TpoDocum>CHEQUE</TpoDocum>
          <Numero>1233456</Numero>
          <Debe>300000</Debe>
        </Movimientos>
        <Movimientos>
          <CodigoCuenta>1.10</CodigoCuenta>
          <TpoDocum>EFECTIVO</TpoDocum>
          <Debe>40658</Debe>
        </Movimientos>
        <ValorComprobante>0</ValorComprobante>
      </Comprobante>
    </LceDiario>
  </LCE>
</LceEnvioLibros>

```


