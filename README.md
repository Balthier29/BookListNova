📚 BookList — Gestor de Libros Interactivo con Vue.js

Proyecto realizado para resolver la problematica que tenia nuestro cliente Editorial Nova, donde generamos una SPA de forma modular y reactiva, cumpliendo con altos estandares de la industria actual.

-----------------------------------------------------------------------------------------------------
📝 Descripción

Booklist es una SPA, construida bajo el motor de VUE 3 Cli, para la gestión interna del catálogo de publicaciones de la editorial Nova, permite registrar, visualizar, filtrar, editar y eliminar Libros. Además de un dashboard con registro y inicio de sesión de usuarios.

-----------------------------------------------------------------------------------------------------
🚀 Tecnologías utilizadas
* Vue 3 cli
* Vue Router (rutas estáticas y dinámicas)

-----------------------------------------------------------------------------------------------------
✨ Funcionalidades principales

* Gestión de libros (CRUD): agregar, editar, listar y eliminar títulos del catálogo.
* Formulario reactivo: campos input, select y textarea vinculados con v-model, actualizando en tiempo real.
* Filtros: se pueden filtrar por autor, categoría y estado de la publicación.
* Edición inline: cada tarjeta del libro se puede actualizar directamente desde la tarjeta creada en /libros.
* Rutas dinámicas: vista de detalle individual por libro (/libros/:id), con props desde router.
* Registro e inicio de sesión de usuarios: tenemos la opcion de registrar mediante formulario indicando nombre y mail. para ingresar a nuestro sistema debe ingresar mediante un mail valido para tener acceso al panel de control.
* Catálogo público: tanto en la pantalla principal y cuando se esta logueado tendra acceso al carrusel con los libros publicados y en estados pendientes en nuestra editorial.
* Diseño actualizado: posee paleta de colores vivos  y componentes reutilizables.

-----------------------------------------------------------------------------------------------------
🗂️ Estructura del proyecto

src/
├── assets/            # Imágenes, íconos y recursos estáticos
├── components/        # Componentes reutilizables
│   ├── headLook.vue
│   ├── FooterLook.vue
│   ├── FormularioLibro.vue
│   ├── LibroItem.vue
│   └── ResumenEstado.vue
├── views/              # Vistas asociadas a rutas
│   ├── inicioHome.vue
│   ├── listaLibros.vue
│   ├── detalleLibro.vue
│   ├── RegistroUsuario.vue
│   └── CatalogoPublico.vue
├── router/
│   └── index.js        # Configuración de Vue Router
├── App.vue              # Componente raíz (estado global, layout)
└── main.js

-----------------------------------------------------------------------------------------------------
🧩 Rutas

Ruta	            Vista	            Descripción
/	                inicioHome	        Landing / panel de bienvenida
/catalogo	        CatalogoPublico	    Vitrina pública de libros (carrusel)
/libros	            listaLibros	        Panel de gestión del catálogo
/libros/:id	        detalleLibro	    Detalle individual de un libro (ruta dinámica)
/registro	        RegistroUsuario	    Registro e inicio de sesión

-----------------------------------------------------------------------------------------------------
⚙️ Instalación y ejecución local

# Clonar el repositorio
git clone [https://github.com/Balthier29/BookListNova]

# Entrar a la carpeta del proyecto
cd proyecto-biblo

# Instalar dependencias
npm install

# Levantar el servidor de desarrollo
npm run serve

El proyecto quedará disponible en http://localhost:8080/ (o el puerto que indique la consola).

-----------------------------------------------------------------------------------------------------

📌 Notas

* El modelo de datos (libros y usuarios) se maneja en memoria dentro de App.vue, sin persistencia en base de datos ni backends.
* actualizar el navegador limpiara la memoria y los datos recien almacenados seran eliminados, esto incluye el usuario y mail, y los libros registrados.
* solo los libros almacenados dentro del data en app.vue se encuentran con imagen de portada, cualquier nuevo libro incluira por defecto la "libroSinImagen" como diseño de portada.
* se ha liberado la opción de catálogo por defecto para cualquiera que revise la página estando o no logeado en el sistema.

-----------------------------------------------------------------------------------------------------

👤 Autor

Davis Vicencio // alias Balthier29.


