let
    // Paso 1: Tabla original
    Origen = Table.FromRows(
        Json.Document(
            Binary.Decompress(
                Binary.FromText(
                    "XZBBasMwEEWvMmgdB0m2WndpJ4GWNBAaly5MFoqihcCWjGxBr5Mz9Ai+WEcOhai7mQeP/2faljCyIvAuh8kNcPQOmAAkG9cPYZLKzD8WV8YpXVOKE6e8yCjLqCDnVUs4ooMLo4Y3K7v51l+8UQ6hVEqPzhs3Rqn8J5eLnMfoRqtOXh0ctJpv9i4fPz53dYXDi0hFxhexWFKtmZyHYh/7qrRvIZK6PKP5IoqYWAXsGDrp9Qh1g6QKV+PuV6YWo4v1hOh02sLue9Le4ouaGh5bsjzx8r/nPCP60hcle3jdxpzHn5QideJp518=",
                    BinaryEncoding.Base64
                ),
                Compression.Deflate
            )
        ),
        let
            TipoTexto = ((type nullable text) meta [Serialized.Text = true])
        in
            type table [
                id_venta = TipoTexto,
                nombre_producto = TipoTexto,
                categoria = TipoTexto,
                precio = TipoTexto,
                fecha_venta = TipoTexto
            ]
    ),

    // Paso 2: Quitar espacios del nombre del producto
    LimpiarEspacios = Table.TransformColumns(
        Origen,
        {{"nombre_producto", Text.Trim}}
    ),

    // Paso 3: Ordenar mayúsculas y minúsculas de la categoría
    EstandarizarCategoria = Table.TransformColumns(
        LimpiarEspacios,
        {{"categoria", Text.Proper}}
    ),

    // Paso 4: Eliminar las filas de prueba
    EliminarPruebas = Table.SelectRows(
        EstandarizarCategoria,
        each [categoria] <> "Prueba"
    ),

    // Paso 5: Cambiar los tipos de datos
    TiparColumnas = Table.TransformColumnTypes(
        EliminarPruebas,
        {
            {"id_venta", Int64.Type},
            {"nombre_producto", type text},
            {"categoria", type text},
            {"precio", type number},
            {"fecha_venta", type date}
        }
    )
in
    TiparColumnas
