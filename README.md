<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Sistema de Facturación Básico</title>
</head>
<body>

    <h1>Sistema de Facturación</h1>

    <form id="formFactura">
        <!-- 1. Datos de la Empresa (Emisor) -->
        <fieldset>
            <legend>Datos de la Empresa</legend>
            <label>Nombre / Razón Social:</label>
            <input type="text" name="emisor_nombre" value="Mi Empresa S.A. de C.V." required><br><br>
            
            <label>RFC:</label>
            <input type="text" name="emisor_rfc" value="EMP010101AAA" required><br><br>
        </fieldset>

        <br>

        <!-- 2. Datos del Cliente (Receptor) -->
        <fieldset>
            <legend>Datos del Cliente</legend>
            <label>Nombre o Razón Social:</label>
            <input type="text" id="cliente_nombre" required><br><br>

            <label>RFC del Cliente:</label>
            <input type="text" id="cliente_rfc" required><br><br>

            <label>Fecha:</label>
            <input type="date" id="fecha" required><br><br>
        </fieldset>

        <br>

        <!-- 3. Conceptos / Productos -->
        <fieldset>
            <legend>Detalle de Productos / Servicios</legend>
            
            <div>
                <label>Cantidad:</label>
                <input type="number" id="cantidad" min="1" value="1">
                
                <label>Descripción:</label>
                <input type="text" id="descripcion" placeholder="Ej. Desarrollo web">
                
                <label>Precio Unitario ($):</label>
                <input type="number" id="precio" step="0.01" min="0" value="0">
                
                <button type="button" onclick="agregarConcepto()">Agregar</button>
            </div>

            <br>

            <!-- Tabla cruda para listar lo que se va agregando -->
            <table border="1" width="100%">
                <thead>
                    <tr>
                        <th>Cant.</th>
                        <th>Descripción</th>
                        <th>Precio Unitario</th>
                        <th>Importe</th>
                        <th>Acción</th>
                    </tr>
                </thead>
                <tbody id="tablaConceptos">
                    <!-- Aquí se insertan las filas dinámicamente -->
                </tbody>
            </table>
        </fieldset>

        <br>

        <!-- 4. Totales -->
        <fieldset>
            <legend>Totales</legend>
            <p>Subtotal: $<span id="subtotal">0.00</span></p>
            <p>IVA (16%): $<span id="iva">0.00</span></p>
            <p><strong>Total a Pagar: $<span id="total">0.00</span></strong></p>
        </fieldset>

        <br>

        <button type="submit">Generar Factura</button>
    </form>

    <!-- Lógica básica en JS para calcular y agregar filas -->
    <script>
        let subtotalGeneral = 0;

        function agregarConcepto() {
            const cantidad = parseFloat(document.getElementById('cantidad').value);
            const descripcion = document.getElementById('descripcion').value;
            const precio = parseFloat(document.getElementById('precio').value);

            if (!descripcion || cantidad <= 0 || precio < 0) {
                alert("Llena bien los datos del producto.");
                return;
            }

            const importe = cantidad * precio;
            subtotalGeneral += importe;

            // Crear fila en la tabla
            const tbody = document.getElementById('tablaConceptos');
            const fila = document.createElement('tr');

            fila.innerHTML = `
                <td>${cantidad}</td>
                <td>${descripcion}</td>
                <td>$${precio.toFixed(2)}</td>
                <td>$${importe.toFixed(2)}</td>
                <td><button type="button" onclick="eliminarFila(this, ${importe})">Quitar</button></td>
            `;

            tbody.appendChild(fila);
            actualizarTotales();

            // Limpiar campos
            document.getElementById('descripcion').value = '';
            document.getElementById('precio').value = '0';
            document.getElementById('cantidad').value = '1';
        }

        function eliminarFila(boton, importe) {
            const fila = boton.parentNode.parentNode;
            fila.remove();
            subtotalGeneral -= importe;
            actualizarTotales();
        }

        function actualizarTotales() {
            const iva = subtotalGeneral * 0.16;
            const total = subtotalGeneral + iva;

            document.getElementById('subtotal').innerText = subtotalGeneral.toFixed(2);
            document.getElementById('iva').innerText = iva.toFixed(2);
            document.getElementById('total').innerText = total.toFixed(2);
        }

        // Evitar que recargue la página al enviar (simulación)
        document.getElementById('formFactura').addEventListener('submit', function(e) {
            e.preventDefault();
            alert("Factura generada correctamente (simulación en frontend).");
        });
    </script>

</body>
</html>
