<template>
  <div>
    <form class="formulario" @submit.prevent="agregarEmpleado">
      <h2>Detalle</h2>
      <div class="campo">
        <label for="dniBeneficiario">Cedula:</label>
        <input id="dniBeneficiario" v-model="detalle.dniBeneficiario" type="text" />
      </div>
      <div class="campo">
        <label for="numeroCuenta">Número Cuenta:</label>
        <input id="numeroCuenta" v-model="detalle.numeroCuenta" type="number" />
      </div>
      <div class="campo">
        <label for="tipoCuenta">Tipo de Cuenta:</label>
        <input id="tipoCuenta" v-model="detalle.tipoCuenta" type="text" />
      </div>
      <div class="campo">
        <label for="numeroRuta">Número de Ruta:</label>
        <input id="numeroRuta" v-model="detalle.numeroRuta" type="number" />
      </div>
      <div class="campo">
        <label for="montoTotal">Monto Total:</label>
        <input id="montoTotal" v-model="detalle.montoTotal" type="number" />
      </div>

      <button type="submit">Agregar Empleado</button>
    </form>

    <button @click="generarArchivoTexto">Generar Archivo de Texto</button>
    <h2>Empleados</h2>
    <table class="tabla-empleados">
      <thead>
      <tr>
        <th>DNI Beneficiario</th>
        <th>Número de Cuenta</th>
        <th>Tipo de cuenta</th>
        <th>Total</th>
        <th>Acciones</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="(empleado, index) in empleados" :key="index">
        <td>{{ empleado.dniBeneficiario }}</td>
        <td>{{ empleado.numeroCuenta }}</td>
        <td>{{ empleado.tipoCuenta }}</td>
        <td>{{ empleado.montoTotal }}</td>
        <td>
          <button @click="eliminarEmpleado(index)">Eliminar</button>
        </td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      detalle: {
        dniBeneficiario: '',
        numeroCuenta: 0,
        tipoCuenta: '',
        numeroRuta: 0,
        montoTotal: 0,
      },
      empleados: [],
    }
  },
  methods: {
    agregarEmpleado() {
      this.empleados.push({ ...this.detalle })

      this.detalle = {
        dniBeneficiario: '',
        numeroCuenta: 0,
        tipoCuenta: '',
        numeroRuta: 0,
        montoTotal: 0,
      }
    },

    eliminarEmpleado(index) {
      this.empleados.splice(index, 1);
    },

    generarArchivoTexto() {
      if (this.empleados.length === 0) {
        alert('No hay empleados para generar el archivo.');
        return;
      }

      let contenidoArchivo = 'DNI Beneficiario,Número de Cuenta,Tipo de Cuenta,Número de Ruta,Monto Total\n';

      this.empleados.forEach((empleado) => {
        contenidoArchivo += `${empleado.dniBeneficiario},${empleado.numeroCuenta},${empleado.tipoCuenta},${empleado.numeroRuta},${empleado.montoTotal}\n`;
      });

      const blob = new Blob([contenidoArchivo], { type: 'text/plain' });

      const url = window.URL.createObjectURL(blob);

      const a = document.createElement('a');
      a.href = url;
      a.download = 'empleados.txt';
      a.click();

      window.URL.revokeObjectURL(url);
    },
  },
}
</script>

<style>
.formulario {
  width: 400px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #f9f9f9;
}

.campo {
  margin-bottom: 10px;
}

.campo input {
  margin-left: 10px;
}

button[type="submit"] {
  background-color: #007bff;
  color: #fff;
  border: none;
  padding: 10px 20px;
  cursor: pointer;
}

.lista-empleados {
  list-style: none;
  padding: 0;
}

.lista-empleados li {
  margin-bottom: 5px;
}

.tabla-empleados {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

.tabla-empleados th,
.tabla-empleados td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

.tabla-empleados th {
  background-color: #f2f2f2;
}

.tabla-empleados tr:hover {
  background-color: #f5f5f5;
}

button {
  background-color: #007bff;
  color: #fff;
  border: none;
  padding: 10px 20px;
  cursor: pointer;
}
</style>