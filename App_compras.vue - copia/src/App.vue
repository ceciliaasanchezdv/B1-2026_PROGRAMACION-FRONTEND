<template>
  <!--Contenedor principal de la aplicación. Aquí estará todo el carrito de compras-->
  <main class="contenedor">

    <!--Uso Bootstrap para crear dos columnas
      row: crea una fila
      col-12: en pantallas pequeñas ocupa todo el ancho
      col-md-6: en pantallas medianas o grandes ocupa la mitad-->
    <div class="row">

      <!--COLUMNA DE PRODUCTOS DISPONIBLES-->
      <section class="col-12 col-md-6">
        <h2 class="disponibles-columna">Productos disponibles</h2>

        <!--v-for recorre el arreglo productos
          producto representa cada producto individual
          productos es el arreglo que tenemos en data()-->
        <div
          class="producto"
          v-for="producto in productos"
          :key="producto.id"
        >

          <!--Muestra la imagen del producto
            :src significa que la ruta viene desde JavaScript
            :alt muestra un texto alternativo con el nombre del producto-->
          <img
            class="imagen-producto"
            :src="producto.imagen"
            :alt="producto.nombre"
          >

          <!--Información del producto-->
          <div class="info-producto">
            <!--Las dobles llaves {{ }} permiten mostrar datos de Vue dentro del HTML-->
            <p class="texto-producto">
              <strong>{{ producto.nombre }}</strong>
              - Precio: $ {{ producto.precio }}
            </p>

            <!--@click ejecuta una función cuando el usuario presiona el botón
                En este caso enviamos el producto actual al método agregarAlCarrito()-->
            <button
              class="btn boton"
              @click="agregarAlCarrito(producto)"
            >
              <i class="bi bi-cart-fill"></i>
              Agregar al carrito
            </button>

          </div>
        </div>
      </section>

      <!--COLUMNA DE PRODUCTOS EN EL CARRITO-->
      <section class="col-12 col-md-6">
        <h2 class="titulo-columna">Productos en el carrito</h2>

        <!--Este mensaje aparece solo si el carrito está vacío-->
        <p
          v-if="carrito.length === 0"
          class="mensaje-vacio">
          No hay productos en el carrito.
        </p>

        <!--Recorre el arreglo carrito para mostrar los productos que el usuario ha agregado-->
        <div
          class="producto-carrito"
          v-for="item in carrito"
          :key="item.id"
        >

          <!--Imagen del producto agregado al carrito-->
          <img
            class="imagen-producto"
            :src="item.imagen"
            :alt="item.nombre"
          >

          <div class="info-producto">
            <!--Muestra nombre y cantidad-->
            <p class="texto-producto">
              <strong>{{ item.nombre }}</strong>
              - Cantidad: {{ item.cantidad }}
            </p>

            <!--Botón para eliminar el producto del carrito-->
            <button
              class="btn boton"
              @click="removerDelCarrito(item.id)"
            >
              <i class="bi bi-trash-fill"></i>
              Remover del carrito
            </button>

          </div>
        </div>
      </section>

    </div>

    <!--Total a pagar.
      Este valor viene de la propiedad computed totalPagar
      Se actualiza automáticamente cada vez que cambia el carrito-->
    <h3 class="total">
      Total a pagar: ${{ totalPagar }}
    </h3>

  </main>
</template>

<script>
export default {
  /*Nombre del componente principal*/
  name: 'App',

  /*data() contiene la información principal de la aplicación
    productos: la lista de productos disponibles
    carrito: los productos que el usuario agrega*/
  data() {
    return {
      /*Lista de productos con precio, stock e imagen*/
      
      productos: [
        {
          id: 1,
          nombre: 'Audífono',
          precio: 30000,
          stock: 3,
          imagen: '/img/audifono.png'
        },
        {
          id: 2,
          nombre: 'Mouse',
          precio: 20000,
          stock: 5,
          imagen: '/img/mouse.png'
        },
        {
          id: 3,
          nombre: 'Teclado',
          precio: 15000,
          stock: 10,
          imagen: '/img/teclado.png'
        },
        {
          id: 4,
          nombre: 'Gabinete',
          precio: 35000,
          stock: 4,
          imagen: '/img/gabinete.png'
        },
        {
          id: 5,
          nombre: 'Pantalla',
          precio: 175000,
          stock: 3,
          imagen: '/img/pantalla.png'
        },
        {
          id: 6,
          nombre: 'Silla',
          precio: 150000,
          stock: 2,
          imagen: '/img/silla.png'
        }
      ],

      /*El carrito inicia vacío
        Aquí se guardarán los productos cuando el usuario presione "Agregar al carrito"*/
      carrito: []
    }
  },

  computed: {
    /*calcula automáticamente el total a pagar*/
    totalPagar() {
      /*reduce recorre el carrito y suma los subtotales
        total empieza en 0*/
      return this.carrito.reduce((total, item) => {
        return total + item.precio * item.cantidad
      }, 0)
    }
  },

  /*methods contiene las funciones que se ejecutan cuando el usuario interactúa con la app*/
  methods: {
    /*Agrega productos al carrito. Recibe como parámetro el producto que el usuario seleccionó*/
    agregarAlCarrito(producto) {
      /*find busca dentro del arreglo carrito
        Si encuentra el producto, lo devuelve
        Si no lo encuentra, devuelve undefined*/
      const productoEnCarrito = this.carrito.find((item) => {
        return item.id === producto.id
      })

      /*Si productoEnCarrito existe, significa que el producto ya fue agregado*/
      if (productoEnCarrito) {
        /*Validamos el stock
          Si la cantidad actual ya es igual o mayor al stock, no se permite agregar más unidades*/
        if (productoEnCarrito.cantidad >= producto.stock) {
          alert('No hay más unidades disponibles en stock.')
          return
        }

        /*Si todavía hay stock disponible, aumentamos la cantidad en 1*/
        productoEnCarrito.cantidad++
      }

      else {
        /*Si el producto no existe en el carrito, lo agregamos con cantidad 1*/
        this.carrito.push({
          id: producto.id,
          nombre: producto.nombre,
          precio: producto.precio,
          imagen: producto.imagen,
          cantidad: 1
        })
      }
    },

    /*Elimina un producto del carrito. Recibe el id del producto que se quiere eliminar*/
    removerDelCarrito(idProducto) {
      /*filter crea un nuevo arreglo.
        Deja en el carrito todos los productos cuyo id sea diferente al id del producto a eliminar*/
      this.carrito = this.carrito.filter((item) => {
        return item.id !== idProducto
      })
    }
  }
}
</script>
