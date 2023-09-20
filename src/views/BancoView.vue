<template>
  <div>
    <h1>Tabla de Empleados desde Archivo</h1>
    <input type="file" @change="cargarArchivo" accept=".txt" />
    <table v-if="empleados.length > 0" class="tabla-empleados">
      <thead>
      <tr>
        <th>DNI Beneficiario</th>
        <th>Número de Cuenta</th>
        <th>Tipo de Cuenta</th>
        <th>Número de Ruta</th>
        <th>Monto Total</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="(empleado, index) in empleados" :key="index">
        <td>{{ empleado.dniBeneficiario }}</td>
        <td>{{ empleado.numeroCuenta }}</td>
        <td>{{ empleado.tipoCuenta }}</td>
        <td>{{ empleado.numeroRuta }}</td>
        <td>{{ empleado.montoTotal }}</td>
      </tr>
      </tbody>

      <tfoot>
      <tr>
        <td colspan="4">Total de Registros:</td>
        <td>{{ totalRegistros }}</td>
      </tr>
      <tr>
        <td colspan="4">Monto Total:</td>
        <td>{{ calcularMontoTotal() }}</td>
      </tr>
      </tfoot>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      empleados: [],
    };
  },
  computed: {
    totalRegistros() {
      return this.empleados.length;
    },
  },
  methods: {
    cargarArchivo(event) {
      const archivo = event.target.files[0];

      if (archivo) {
        const lector = new FileReader();

        lector.onload = (e) => {
          const contenido = e.target.result;
          this.procesarContenido(contenido);
        };

        lector.readAsText(archivo);
      } else {
        this.empleados = [];
      }
    },
    procesarContenido(contenido) {
      const lineas = contenido.split('\n');
      const empleados = [];

      lineas.forEach((linea, index) => {
        if (index === 0) {
          return;
        }

        const campos = linea.split(',');
        if (campos.length === 5) {
          const [dniBeneficiario, numeroCuenta, tipoCuenta, numeroRuta, montoTotal] = campos;
          empleados.push({
            dniBeneficiario,
            numeroCuenta,
            tipoCuenta,
            numeroRuta,
            montoTotal,
          });
        }
      });

      this.empleados = empleados;
    },
    calcularMontoTotal() {
      return this.empleados.reduce((total, empleado) => {
        return total + parseInt(empleado.montoTotal);
      }, 0);
    },
  },
};
</script>

<style scoped>
.tabla-empleados {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

.tabla-empleados th, .tabla-empleados td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

.tabla-empleados th {
  background-color: #f2f2f2;
}

.tabla-empleados tr:nth-child(even) {
  background-color: #f2f2f2;
}

.tabla-empleados tr:hover {
  background-color: #ddd;
}
</style>