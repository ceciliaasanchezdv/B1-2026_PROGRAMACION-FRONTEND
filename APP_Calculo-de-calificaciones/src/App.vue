<template>
  <!-- Contenedor principal. La clase container viene de Bootstrap y ayuda a que sea responsivo-->
  <main class="container contenedor-principal mt-5">

    <h1 class="text-center mb-4">
      Programación Front End
    </h1>

    <!-- Card de Bootstrap-->
    <div class="card shadow">
      <!-- Encabezado de la tarjeta -->
      <div class="card-header">
        <!-- Barra de navegación con pestañas-->
        <ul class="nav nav-tabs card-header-tabs">
          <!-- Pestaña 1: Cálculo de calificaciones -->
          <li class="nav-item">
            <!-- Botón de la pestaña Cálculo de calificaciones 
             :class agrega la clase active solo si esta pestaña está seleccionada
             @click cambia la pestaña activa cuando el usuario hace clic-->
            <button class="nav-link" :class="{ active: pestanaActiva === 'calificaciones' }"
              @click="pestanaActiva = 'calificaciones'">
              Cálculo de calificaciones
            </button>
          </li>

          <!-- Pestaña 2: Formulario de registro -->
          <li class="nav-item">
            <!-- Botón de la pestaña Formulario de registro -->
            <button class="nav-link" :class="{ active: pestanaActiva === 'registro' }"
              @click="pestanaActiva = 'registro'">
              Formulario de registro
            </button>
          </li>
        </ul>
      </div>

      <!-- Cuerpo de la tarjeta -->
      <div class="card-body">
        <!-- PRIMERA PESTAÑA
         v-if muestra esta sección solo cuando la pestaña activa sea "calificaciones"-->
        <section v-if="pestanaActiva === 'calificaciones'">
          <h2>Cálculo de calificaciones</h2>
          <p>
            Ingresa las notas y la asistencia para saber si el estudiante aprueba.
          </p>

          <!-- Formulario de notas 
           @submit.prevent evita que la página se recargue-->
          <form class="formulario-notas" @submit.prevent>
            <!-- Campo Nota 1 -->
            <div class="campos">
              <label class="form-label">Nota 1</label>
              <!-- v-model conecta este input con la variable nota1-->
              <input type="number" class="form-control" v-model="nota1" min="10" max="70" required
                placeholder="Ingrese nota entre 10 y 70">
            </div>

            <!-- Campo Nota 2 -->
            <div class="campos">
              <label class="form-label">Nota 2</label>
              <input type="number" class="form-control" v-model="nota2" min="10" max="70" required
                placeholder="Ingrese nota entre 10 y 70">
            </div>

            <!-- Campo Nota 3 -->
            <div class="campos">
              <label class="form-label">Nota 3</label>
              <input type="number" class="form-control" v-model="nota3" min="10" max="70" required
                placeholder="Ingrese nota entre 10 y 70">
            </div>

            <!-- Campo Asistencia -->
            <div class="campos">
              <label class="form-label">Asistencia (%)</label>
              <input type="number" class="form-control" v-model="asistencia" min="0" max="100" required
                placeholder="Ingrese asistencia entre 0 y 100">
            </div>

            <!-- Botón para calcular -->
            <button class="btn botones" type="submit" @click="calcularCalificaciones">
              Calcular
            </button>

            <!-- Mensaje general de error con Vue -->
            <p class="text-danger mt-3 text-center" v-if="mensajeError">
              {{ mensajeError }}
            </p>

          </form>

          <!-- Resultado del cálculo -->
          <div v-if="mensajeResultado" class="alert mt-4 text-center"
            :class="aprobado ? 'alert-success' : 'alert-danger'">
            <strong>Promedio:</strong> {{ promedio }} <br>
            <strong>Resultado:</strong> {{ mensajeResultado }}
          </div>
        </section>

        <!-- SEGUNDA PESTAÑA -->
        <!-- v-if muestra esta sección solo cuando la pestaña activa sea "registro"-->
        <section v-if="pestanaActiva === 'registro'">
          <h2>Formulario de registro</h2>

          <p>
            Completa los datos para realizar el registro.
          </p>

          <!-- Formulario de registro -->
          <form class="formulario-notas" @submit.prevent="enviarRegistro">

            <!-- Campo Nombre -->
            <div class="campos">
              <label class="form-label">Nombre</label>
              <input type="text" class="form-control" v-model="nombre" placeholder="Ingrese su nombre">

              <p class="text-danger" v-if="errorNombre">
                {{ errorNombre }}
              </p>
            </div>

            <!-- Campo Correo -->
            <div class="campos">
              <label class="form-label">Correo</label>
              <input type="text" class="form-control" v-model="correo" placeholder="Ingrese su correo">

              <p class="text-danger" v-if="errorCorreo">
                {{ errorCorreo }}
              </p>
            </div>

            <!-- Campo Contraseña -->
            <div class="campos">
              <label class="form-label">Contraseña</label>
              <input type="password" class="form-control" v-model="contrasena" placeholder="Ingrese una contraseña">

              <p class="text-danger" v-if="errorContrasena">
                {{ errorContrasena }}
              </p>
            </div>

            <!-- Campo Repetir Contraseña -->
            <div class="campos">
              <label class="form-label">Repetir contraseña</label>
              <input type="password" class="form-control" v-model="repetirContrasena"
                placeholder="Repita la contraseña">

              <p class="text-danger" v-if="errorRepetirContrasena">
                {{ errorRepetirContrasena }}
              </p>
            </div>

            <!-- Botón enviar -->
            <button class="btn botones" type="submit">
              Enviar
            </button>

          </form>
        </section>

      </div>
    </div>
  </main>
</template>

<script setup>
// Importamos ref desde Vue. ref permite crear variables reactivas, es decir, variables que actualizan la vista.
import { ref } from 'vue'

// Variable que indica cuál pestaña se está mostrando. Por defecto mostramos la pestaña de calificaciones
const pestanaActiva = ref('calificaciones')

// Variables para guardar las notas y la asistencia. Se dejan como texto vacío
const nota1 = ref('')
const nota2 = ref('')
const nota3 = ref('')
const asistencia = ref('')

// Variables para mensajes y resultado
const mensajeError = ref('')
const promedio = ref('')
const mensajeResultado = ref('')
const aprobado = ref(false)

// Variables del formulario de registro
const nombre = ref('')
const correo = ref('')
const contrasena = ref('')
const repetirContrasena = ref('')

// Variables para mostrar errores del formulario de registro
const errorNombre = ref('')
const errorCorreo = ref('')
const errorContrasena = ref('')
const errorRepetirContrasena = ref('')

// Función que se ejecuta al presiona el botón Calcular
function calcularCalificaciones() {
  // Limpiamos errores anteriores
  mensajeError.value = ''
  promedio.value = ''
  mensajeResultado.value = ''
  aprobado.value = false

  // Convertimos los valores ingresados a número
  const n1 = Number(nota1.value)
  const n2 = Number(nota2.value)
  const n3 = Number(nota3.value)
  const asist = Number(asistencia.value)

  // Validamos que los campos tengan valores correctos
  if (
    nota1.value === '' ||
    nota2.value === '' ||
    nota3.value === '' ||
    asistencia.value === '' ||
    n1 < 10 || n1 > 70 ||
    n2 < 10 || n2 > 70 ||
    n3 < 10 || n3 > 70 ||
    asist < 0 || asist > 100
  ) {
    mensajeError.value = 'Por favor, ingrese valores válidos para las notas y la asistencia'
    return
  }

  // Calculamos el promedio ponderado
  // Nota 1 vale 35%, Nota 2 vale 35% y Nota 3 vale 30%
  const resultado = (n1 * 0.35) + (n2 * 0.35) + (n3 * 0.30)

  // Mostramos el promedio con un decimal
  promedio.value = resultado.toFixed(1)

  // Para aprobar debe tener promedio 40 o más y asistencia mínima 80%
  if (resultado >= 40 && asist >= 80) {
    aprobado.value = true
    mensajeResultado.value = 'Aprobado'
  } else {
    aprobado.value = false
    mensajeResultado.value = 'Reprobado'
  }

  // Limpiamos los campos después de calcular
  nota1.value = ''
  nota2.value = ''
  nota3.value = ''
  asistencia.value = ''
}

// Esta función se ejecuta al presionar el botón Enviar
function enviarRegistro() {
  // Limpiamos errores anteriores
  errorNombre.value = ''
  errorCorreo.value = ''
  errorContrasena.value = ''
  errorRepetirContrasena.value = ''

  // Variable para saber si hay errores
  let hayError = false

  // Validamos el nombre
  if (nombre.value === '') {
    errorNombre.value = 'El nombre es obligatorio.'
    hayError = true
  }

  // Validamos el correo
  const formatoCorreo = /^[^\s@]+@[^\s@]+\.[^\s@]+$/

  if (correo.value === '') {
    errorCorreo.value = 'El correo es obligatorio.'
    hayError = true
  } else if (!formatoCorreo.test(correo.value)) {
    errorCorreo.value = 'Ingrese un correo válido.'
    hayError = true
  }

  // Validamos la contraseña
  if (contrasena.value === '') {
    errorContrasena.value = 'La contraseña es obligatoria.'
    hayError = true
  }

  // Validamos repetir contraseña
  if (repetirContrasena.value === '') {
    errorRepetirContrasena.value = 'Debe repetir la contraseña.'
    hayError = true
  } else if (contrasena.value !== repetirContrasena.value) {
    errorRepetirContrasena.value = 'Las contraseñas no coinciden.'
    hayError = true
  }

  // Si hay errores, se detiene la función
  if (hayError) {
    return
  }

  // Si todo está correcto, se muestra el alert
  alert('El registro se ha realizado correctamente')

  // Limpia el formulario después del registro correcto
  nombre.value = ''
  correo.value = ''
  contrasena.value = ''
  repetirContrasena.value = ''
}
</script>