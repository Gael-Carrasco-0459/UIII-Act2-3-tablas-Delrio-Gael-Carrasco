¡Claro! Aquí tienes la primera parte del proyecto "Delrio" con todos los procedimientos y el código solicitado.

### **Proyecto: Delrio**

**Lenguaje:** Python
**Framework:** Django
**Editor:** VS Code

---

### **1. Procedimiento para crear carpeta del Proyecto: UIII_Delrio_0459**

Abre tu terminal (o la terminal de Git Bash si estás en Windows) y ejecuta el siguiente comando:

```bash
mkdir UIII_Delrio_0459
```

---

### **2. Procedimiento para abrir VS Code sobre la carpeta UIII_Delrio_0459**

Desde la misma terminal, navega a la carpeta que acabas de crear y ábrela con VS Code:

```bash
cd UIII_Delrio_0459
code .
```
Esto abrirá VS Code con la carpeta `UIII_Delrio_0459` como el espacio de trabajo principal.

---

### **3. Procedimiento para abrir terminal en VS Code**

Dentro de VS Code, puedes abrir la terminal yendo a `Terminal` -> `New Terminal` en el menú superior, o usando el atajo de teclado `Ctrl + Ñ` (o `Cmd + J` en macOS).

---

### **4. Procedimiento para crear carpeta entorno virtual “.venv” desde terminal de VS Code**

Una vez que tengas la terminal abierta en VS Code y estés en la raíz de tu proyecto (`UIII_Delrio_0459`), ejecuta:

```bash
python -m venv .venv
```
Esto creará una carpeta llamada `.venv` que contendrá tu entorno virtual.

---

### **5. Procedimiento para activar el entorno virtual.**

En la terminal de VS Code:

*   **En Windows (PowerShell):**
    ```bash
    .venv\Scripts\Activate.ps1
    ```
*   **En Windows (CMD):**
    ```bash
    .venv\Scripts\activate.bat
    ```
*   **En macOS/Linux (Bash):**
    ```bash
    source .venv/bin/activate
    ```
Verás `(.venv)` al inicio de tu prompt de la terminal, indicando que el entorno virtual está activo.

---

### **6. Procedimiento para activar intérprete de python.**

Cuando el entorno virtual está activo, VS Code debería detectar automáticamente el intérprete de Python dentro de `.venv`. Si no lo hace, puedes seleccionarlo manualmente:

1.  Abre la paleta de comandos (`Ctrl + Shift + P` o `Cmd + Shift + P`).
2.  Escribe "Python: Select Interpreter".
3.  Selecciona la opción que apunte a `Python 3.x.x (.venv)`.

---

### **7. Procedimiento para instalar Django**

Con el entorno virtual activado, instala Django usando pip:

```bash
pip install Django
```

---

### **8. Procedimiento para crear proyecto backend_Delrio sin duplicar carpeta.**

Asegúrate de estar en la raíz de tu proyecto (`UIII_Delrio_0459`) con el entorno virtual activado. Luego, crea el proyecto Django:

```bash
django-admin startproject backend_Delrio .
```
El `.` al final es crucial para que Django cree el proyecto en la carpeta actual sin una subcarpeta extra.

---

### **9. Procedimiento para ejecutar servidor en el puerto 8459**

Para iniciar el servidor de desarrollo de Django en el puerto especificado:

```bash
python manage.py runserver 8459
```

---

### **10. Procedimiento para copiar y pegar el link en el navegador.**

Cuando el servidor se esté ejecutando, la terminal te mostrará un enlace como este:

```
Starting development server at http://127.0.0.1:8459/
Quit the server with CONTROL-C.
```
Copia `http://127.0.0.1:8459/` y pégalo en tu navegador web. Deberías ver la página de bienvenida de Django.

---

### **11. Procedimiento para crear aplicación app_Delrio**

Detén el servidor (Ctrl+C en la terminal). Luego, crea la aplicación dentro de tu proyecto Django:

```bash
python manage.py startapp app_Delrio
```
Esto creará una nueva carpeta `app_Delrio` con la estructura básica de una aplicación Django.

---

### **12. Aquí el modelo models.py**

Abre el archivo `app_Delrio/models.py` y reemplaza su contenido con el siguiente código:

```python
# app_Delrio/models.py
from django.db import models

# ==========================================
# MODELO: CLIENTE
# ==========================================
class Cliente(models.Model):
    nom_clie = models.CharField(max_length=100)
    apellido_clie = models.CharField(max_length=100)
    C_E_clie = models.CharField(max_length=100)  # Correo electrónico
    tel_clie = models.CharField(max_length=20)
    direc_clie = models.TextField()
    fech_compra = models.DateField()

    def __str__(self):
        return f"{self.nom_clie} {self.apellido_clie}"


# ==========================================
# MODELO: PRODUCTO
# ==========================================
class Producto(models.Model):
    nom_prod = models.CharField(max_length=100)
    desc_prod = models.TextField(blank=True, null=True)
    precio_unidad = models.DecimalField(max_digits=10, decimal_places=2)
    stock = models.IntegerField()
    fech_creacion = models.DateTimeField(auto_now_add=True)
    id_prov = models.IntegerField()  # Si existiera tabla 'proveedor', sería ForeignKey

    def __str__(self):
        return self.nom_prod


# ==========================================
# MODELO: VENTA
# ==========================================
class Venta(models.Model):
    fech_venta = models.DateTimeField(auto_now_add=True)
    total_venta = models.DecimalField(max_digits=10, decimal_places=2)
    estado = models.CharField(max_length=50)

    # Relaciones foráneas
    id_clie = models.ForeignKey(Cliente, on_delete=models.CASCADE, related_name="ventas")
    id_prod = models.ForeignKey(Producto, on_delete=models.CASCADE, related_name="ventas")
    id_empl = models.IntegerField()  # Si tuvieras tabla Empleado, usarías ForeignKey

    def __str__(self):
        return f"Venta #{self.id} - Cliente: {self.id_clie.nom_clie} - Total: ${self.total_venta}"
```

---

### **12.5 Procedimiento para realizar las migraciones (makemigrations y migrate)**

Antes de migrar, necesitamos registrar nuestra aplicación en el archivo `settings.py` del proyecto principal.

Abre `backend_Delrio/settings.py` y añade `'app_Delrio'` a la lista `INSTALLED_APPS`:

```python
# backend_Delrio/settings.py

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app_Delrio', # ¡Añade esta línea!
]
```

Ahora, guarda el archivo y en la terminal ejecuta:

```bash
python manage.py makemigrations
python manage.py migrate
```
Esto creará los archivos de migración y aplicará los cambios a la base de datos (SQLite por defecto).

---

### **13. Primero trabajamos con el MODELO: VENTAS**

De acuerdo con las instrucciones, nos enfocaremos en el modelo `Venta` para las operaciones CRUD.

---

### **14. En view de app_Delrio crear las funciones con sus códigos correspondientes (inicio_Delrio, agregar_venta, actualizar_venta, realizar_actualizacion_venta, borrar_venta)**

Abre `app_Delrio/views.py` y reemplaza su contenido con el siguiente código. Ten en cuenta que para las ventas, necesitamos tener al menos un cliente y un producto creados para poder asociarlos. Por ahora, asumiremos que existen.

```python
# app_Delrio/views.py
from django.shortcuts import render, redirect, get_object_or_404
from .models import Venta, Cliente, Producto # Importa los modelos

# ==========================================
# VISTAS PARA VENTA
# ==========================================

def inicio_Delrio(request):
    """
    Vista principal del sistema.
    """
    return render(request, 'inicio.html')

def ver_ventas(request):
    """
    Muestra una lista de todas las ventas.
    """
    ventas = Venta.objects.all().order_by('-fech_venta')
    return render(request, 'Ventas/ver_venta.html', {'ventas': ventas})


def agregar_venta(request):
    """
    Permite agregar una nueva venta.
    """
    clientes = Cliente.objects.all()
    productos = Producto.objects.all()

    if request.method == 'POST':
        try:
            id_clie = request.POST.get('id_clie')
            id_prod = request.POST.get('id_prod')
            total_venta = request.POST.get('total_venta')
            estado = request.POST.get('estado')
            id_empl = request.POST.get('id_empl')

            cliente_obj = get_object_or_404(Cliente, id=id_clie)
            producto_obj = get_object_or_404(Producto, id=id_prod)

            Venta.objects.create(
                id_clie=cliente_obj,
                id_prod=producto_obj,
                total_venta=total_venta,
                estado=estado,
                id_empl=id_empl,
            )
            return redirect('ver_ventas')
        except Exception as e:
            # Aquí puedes manejar errores si los datos no son válidos
            print(f"Error al agregar venta: {e}")
            # Puedes añadir un mensaje de error al contexto para mostrar en la plantilla
            return render(request, 'Ventas/agregar_venta.html', {
                'clientes': clientes,
                'productos': productos,
                'error_message': f"Hubo un error al guardar la venta: {e}"
            })

    return render(request, 'Ventas/agregar_venta.html', {'clientes': clientes, 'productos': productos})


def actualizar_venta(request, pk):
    """
    Muestra el formulario para actualizar una venta existente.
    """
    venta = get_object_or_404(Venta, pk=pk)
    clientes = Cliente.objects.all()
    productos = Producto.objects.all()
    return render(request, 'Ventas/actualizar_venta.html', {
        'venta': venta,
        'clientes': clientes,
        'productos': productos
    })


def realizar_actualizacion_venta(request, pk):
    """
    Procesa la actualización de una venta.
    """
    venta = get_object_or_404(Venta, pk=pk)
    if request.method == 'POST':
        try:
            id_clie = request.POST.get('id_clie')
            id_prod = request.POST.get('id_prod')
            total_venta = request.POST.get('total_venta')
            estado = request.POST.get('estado')
            id_empl = request.POST.get('id_empl')

            venta.id_clie = get_object_or_404(Cliente, id=id_clie)
            venta.id_prod = get_object_or_404(Producto, id=id_prod)
            venta.total_venta = total_venta
            venta.estado = estado
            venta.id_empl = id_empl
            venta.save()
            return redirect('ver_ventas')
        except Exception as e:
            print(f"Error al actualizar venta: {e}")
            clientes = Cliente.objects.all()
            productos = Producto.objects.all()
            return render(request, 'Ventas/actualizar_venta.html', {
                'venta': venta,
                'clientes': clientes,
                'productos': productos,
                'error_message': f"Hubo un error al actualizar la venta: {e}"
            })
    return redirect('ver_ventas') # Redirige si se accede por GET


def borrar_venta(request, pk):
    """
    Borra una venta.
    """
    venta = get_object_or_404(Venta, pk=pk)
    if request.method == 'POST':
        venta.delete()
        return redirect('ver_ventas')
    # Opcional: puedes renderizar una página de confirmación de borrado
    return render(request, 'Ventas/borrar_venta.html', {'venta': venta})
```

---

### **15. Crear la carpeta “templates” dentro de “app_Delrio”.**

En la raíz de tu aplicación `app_Delrio`, crea una nueva carpeta llamada `templates`:

```
UIII_Delrio_0459/
├── backend_Delrio/
├── app_Delrio/
│   ├── migrations/
│   ├── templates/  <-- Aquí
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── tests.py
│   └── views.py
└── manage.py
```

---

### **16. En la carpeta templates crear los archivos html (base.html, header.html, navbar.html, footer.html, inicio.html).**

Dentro de `app_Delrio/templates/`, crea los siguientes archivos vacíos por ahora:

*   `base.html`
*   `header.html`
*   `navbar.html`
*   `footer.html`
*   `inicio.html`

---

### **17. En el archivo base.html agregar bootstrap para css y js.**

Abre `app_Delrio/templates/base.html` y añade el siguiente código. Usaremos Bootstrap 5.3.

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block title %}Sistema Delrio{% endblock %}</title>
    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
    <!-- Font Awesome para iconos -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.2/css/all.min.css" integrity="sha512-SnH5WK+bZxgPHs44uWIX+LLJAJ9/2PkPKZ5QiAj6Ta86w+fsb2TkcmfRyVX3pBnMFcV7oQPJkl9QevSCWr3W6A==" crossorigin="anonymous" referrerpolicy="no-referrer" />
    <style>
        body {
            display: flex;
            flex-direction: column;
            min-height: 100vh;
        }
        .content {
            flex: 1;
            padding-bottom: 70px; /* Espacio para el footer */
        }
        .footer {
            position: fixed;
            bottom: 0;
            width: 100%;
            height: 60px; /* Altura del footer */
            background-color: #f8f9fa; /* Color de fondo del footer */
            border-top: 1px solid #dee2e6;
            text-align: center;
            line-height: 60px; /* Centrar texto verticalmente */
            z-index: 1000;
        }
        .navbar-custom {
            background-color: #6c757d; /* Gris oscuro para la barra de navegación */
        }
        .navbar-brand, .nav-link, .dropdown-item {
            color: #ffffff !important; /* Texto blanco */
        }
        .navbar-brand:hover, .nav-link:hover, .dropdown-item:hover {
            color: #dee2e6 !important; /* Gris claro al pasar el ratón */
        }
        .dropdown-menu {
            background-color: #6c757d;
        }
    </style>
</head>
<body>
    {% include 'navbar.html' %}

    <div class="container mt-4 content">
        {% block content %}
        {% endblock %}
    </div>

    {% include 'footer.html' %}

    <!-- Bootstrap JS y Popper.js -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
    <script>
        // Script para obtener y mostrar la fecha actual
        document.addEventListener('DOMContentLoaded', function() {
            const dateElement = document.getElementById('current-date');
            if (dateElement) {
                const today = new Date();
                const options = { year: 'numeric', month: 'long', day: 'numeric' };
                dateElement.textContent = today.toLocaleDateString('es-ES', options);
            }
        });
    </script>
</body>
</html>
```

---

### **18. En el archivo navbar.html incluir las opciones (...)**

Abre `app_Delrio/templates/navbar.html` y pega el siguiente código:

```html
<!-- app_Delrio/templates/navbar.html -->
<nav class="navbar navbar-expand-lg navbar-dark bg-dark navbar-custom">
    <div class="container-fluid">
        <a class="navbar-brand" href="{% url 'inicio_delrio' %}">
            <i class="fas fa-cubes me-2"></i>Sistema de Administración Delrio
        </a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNavDropdown" aria-controls="navbarNavDropdown" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNavDropdown">
            <ul class="navbar-nav ms-auto">
                <li class="nav-item">
                    <a class="nav-link active" aria-current="page" href="{% url 'inicio_delrio' %}">
                        <i class="fas fa-home me-1"></i>Inicio
                    </a>
                </li>
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                        <i class="fas fa-shopping-cart me-1"></i>Venta
                    </a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="{% url 'agregar_venta' %}">Agregar Venta</a></li>
                        <li><a class="dropdown-item" href="{% url 'ver_ventas' %}">Ver Ventas</a></li>
                        <!-- Los enlaces de actualizar y borrar se manejan desde 'ver_ventas' -->
                    </ul>
                </li>
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                        <i class="fas fa-users me-1"></i>Cliente
                    </a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="#">Agregar Cliente</a></li>
                        <li><a class="dropdown-item" href="#">Ver Clientes</a></li>
                        <li><a class="dropdown-item" href="#">Actualizar Cliente</a></li>
                        <li><a class="dropdown-item" href="#">Borrar Cliente</a></li>
                    </ul>
                </li>
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                        <i class="fas fa-box-open me-1"></i>Producto
                    </a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="#">Agregar Productos</a></li>
                        <li><a class="dropdown-item" href="#">Ver Productos</a></li>
                        <li><a class="dropdown-item" href="#">Actualizar Producto</a></li>
                        <li><a class="dropdown-item" href="#">Borrar Producto</a></li>
                    </ul>
                </li>
            </ul>
        </div>
    </div>
</nav>
```

---

### **19. En el archivo footer.html incluir derechos de autor, fecha del sistema y “Creado por Gael carrasco, Cbtis 128” y mantenerla fija al final de la página.**

Abre `app_Delrio/templates/footer.html` y añade:

```html
<!-- app_Delrio/templates/footer.html -->
<footer class="footer bg-light text-muted py-3">
    <div class="container text-center">
        <span>&copy; 2024 Sistema de Administración Delrio. Todos los derechos reservados.</span> |
        <span>Fecha del Sistema: <span id="current-date"></span></span> |
        <span>Creado por Gael Carrasco, CBTIS 128</span>
    </div>
</footer>
```

---

### **20. En el archivo inicio.html se usa para colocar información del sistema más una imagen tomada desde la red sobre Delrio.**

Abre `app_Delrio/templates/inicio.html` y añade el siguiente contenido. He puesto un placeholder para la imagen.

```html
<!-- app_Delrio/templates/inicio.html -->
{% extends 'base.html' %}

{% block title %}Inicio - Delrio{% endblock %}

{% block content %}
<div class="row">
    <div class="col-12 text-center mb-4">
        <h1 class="display-4 text-primary">Bienvenido al Sistema de Administración Delrio</h1>
        <p class="lead">Gestiona tus ventas, clientes y productos de manera eficiente.</p>
    </div>
</div>

<div class="row justify-content-center mb-5">
    <div class="col-lg-8">
        <div class="card shadow-sm">
            <div class="card-body">
                <h5 class="card-title text-success mb-3"><i class="fas fa-info-circle me-2"></i>Sobre el Sistema Delrio</h5>
                <p class="card-text">
                    Este sistema está diseñado para facilitar la administración de las operaciones de venta de Delrio.
                    Podrás registrar nuevas ventas, mantener un control de los productos y gestionar la información de tus clientes.
                    Nuestro objetivo es ofrecerte una herramienta robusta y fácil de usar para optimizar tus procesos.
                </p>
                <p class="card-text">
                    Utiliza el menú de navegación para acceder a las diferentes secciones: "Inicio", "Venta", "Cliente" y "Producto".
                    En la sección "Venta" podrás realizar las operaciones CRUD (Crear, Leer, Actualizar, Borrar) para el registro de transacciones.
                </p>
                <hr>
                <p class="card-text text-muted">¡Empieza a administrar tu negocio con Delrio hoy mismo!</p>
            </div>
        </div>
    </div>
</div>

<div class="row justify-content-center">
    <div class="col-lg-8 text-center">
        <h4>Nuestra Visión</h4>
        <p>
            En Delrio, nos esforzamos por ser líderes en la gestión de servicios y productos, garantizando la satisfacción de nuestros clientes
            y la eficiencia en cada una de nuestras operaciones.
        </p>
        <div class="mt-4">
            <!-- Imagen representativa de Delrio (reemplaza con una URL real si lo deseas) -->
            <img src="https://picsum.photos/800/400?random=1" class="img-fluid rounded shadow-sm" alt="Imagen representativa de Delrio">
            <small class="text-muted d-block mt-2">Imagen ilustrativa del concepto Delrio.</small>
        </div>
    </div>
</div>
{% endblock %}
```

#### `actualizar_venta.html`

```html
<!-- app_Delrio/templates/Ventas/actualizar_venta.html -->
{% extends 'base.html' %}

{% block title %}Actualizar Venta{% endblock %}

{% block content %}
<div class="row justify-content-center">
    <div class="col-md-8">
        <div class="card shadow-sm">
            <div class="card-header bg-warning text-dark">
                <h3 class="mb-0"><i class="fas fa-edit me-2"></i>Actualizar Venta #{{ venta.id }}</h3>
            </div>
            <div class="card-body">
                {% if error_message %}
                    <div class="alert alert-danger" role="alert">
                        {{ error_message }}
                    </div>
                {% endif %}
                <form action="{% url 'realizar_actualizacion_venta' venta.id %}" method="post">
                    {% csrf_token %}
                    <div class="mb-3">
                        <label for="id_clie" class="form-label">Cliente</label>
                        <select class="form-select" id="id_clie" name="id_clie" required>
                            <option value="">Seleccione un cliente</option>
                            {% for cliente in clientes %}
                                <option value="{{ cliente.id }}" {% if cliente.id == venta.id_clie.id %}selected{% endif %}>
                                    {{ cliente.nom_clie }} {{ cliente.apellido_clie }}
                                </option>
                            {% endfor %}
                        </select>
                    </div>
                    <div class="mb-3">
                        <label for="id_prod" class="form-label">Producto</label>
                        <select class="form-select" id="id_prod" name="id_prod" required>
                            <option value="">Seleccione un producto</option>
                            {% for producto in productos %}
                                <option value="{{ producto.id }}" {% if producto.id == venta.id_prod.id %}selected{% endif %}>
                                    {{ producto.nom_prod }} - ${{ producto.precio_unidad }}
                                </option>
                            {% endfor %}
                        </select>
                    </div>
                    <div class="mb-3">
                        <label for="total_venta" class="form-label">Total Venta</label>
                        <input type="number" step="0.01" class="form-control" id="total_venta" name="total_venta" value="{{ venta.total_venta }}" required>
                    </div>
                    <div class="mb-3">
                        <label for="estado" class="form-label">Estado</label>
                        <input type="text" class="form-control" id="estado" name="estado" value="{{ venta.estado }}" required>
                    </div>
                    <div class="mb-3">
                        <label for="id_empl" class="form-label">ID Empleado</label>
                        <input type="number" class="form-control" id="id_empl" name="id_empl" value="{{ venta.id_empl }}" required>
                    </div>
                    <div class="d-grid gap-2 d-md-flex justify-content-md-end">
                        <button type="submit" class="btn btn-warning me-md-2"><i class="fas fa-sync-alt me-1"></i>Actualizar Venta</button>
                        <a href="{% url 'ver_ventas' %}" class="btn btn-secondary"><i class="fas fa-arrow-left me-1"></i>Cancelar</a>
                    </div>
                </form>
            </div>
        </div>
    </div>
</div>
{% endblock %}
```

#### `borrar_venta.html`

Aunque el borrado se maneja directamente con un modal y POST en `ver_venta.html`, es buena práctica tener una plantilla para una confirmación más explícita o para otros casos. Para este proyecto, el modal es suficiente, pero si quisieras una página separada, el contenido sería similar al del modal. Por el momento, el `views.py` está configurado para que el botón de borrar en `ver_venta.html` envíe un POST directamente. Si quisieras una página de confirmación, `borrar_venta` en `views.py` debería renderizar esta plantilla.

Para este proyecto, el modal de confirmación en `ver_venta.html` es suficiente y más moderno. No necesitamos una plantilla `borrar_venta.html` separada para la confirmación visual si el borrado se hace con el modal. La función `borrar_venta` en `views.py` ya redirige después de borrar.

---

### **23. No utilizar forms.py.**

Confirmado. Estamos manejando los formularios directamente en las plantillas HTML con `request.POST`.

---

### **24. Procedimiento para crear el archivo urls.py en app_Delrio con el código correspondiente para acceder a las funciones de views.py para operaciones de crud en Ventas.**

En la carpeta `app_Delrio`, crea un nuevo archivo llamado `urls.py` y añade el siguiente contenido:

```python
# app_Delrio/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.inicio_Delrio, name='inicio_delrio'),
    path('ventas/', views.ver_ventas, name='ver_ventas'),
    path('ventas/agregar/', views.agregar_venta, name='agregar_venta'),
    path('ventas/actualizar/<int:pk>/', views.actualizar_venta, name='actualizar_venta'),
    path('ventas/actualizar/realizar/<int:pk>/', views.realizar_actualizacion_venta, name='realizar_actualizacion_venta'),
    path('ventas/borrar/<int:pk>/', views.borrar_venta, name='borrar_venta'),
]
```

---

### **25. Procedimiento para agregar app_Delrio en settings.py de backend_Delrio**

(Ya lo hicimos en el punto 12.5 al momento de preparar para las migraciones, pero lo reiteramos aquí para confirmar).

Abre `backend_Delrio/settings.py` y asegúrate de que `'app_Delrio'` esté en la lista `INSTALLED_APPS`:

```python
# backend_Delrio/settings.py

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app_Delrio', # ¡Esta línea debe estar presente!
]
```

---

### **26. Realizar las configuraciones correspondiente a urls.py de backend_Delrio para enlazar con app_Delrio**

Abre `backend_Delrio/urls.py` y añade la inclusión de las URLs de `app_Delrio`:

```python
# backend_Delrio/urls.py
from django.contrib import admin
from django.urls import path, include # Importa include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_Delrio.urls')), # ¡Añade esta línea!
]
```

---

### **27. Procedimiento para registrar los modelos en admin.py y volver a realizar las migraciones.**

Abre `app_Delrio/admin.py` y registra tus modelos. **IMPORTANTE**: Para que el CRUD de Ventas funcione correctamente, necesitamos poder crear Clientes y Productos en el admin, ya que son Foreign Keys de Venta.

```python
# app_Delrio/admin.py
from django.contrib import admin
from .models import Cliente, Producto, Venta

# Registra los modelos aquí.
admin.site.register(Cliente)
admin.site.register(Producto)
admin.site.register(Venta)
```

**Volver a realizar las migraciones:** En este caso, no necesitamos `makemigrations` porque no hemos cambiado la estructura de los modelos desde la última vez. Solo necesitamos aplicar cualquier migración pendiente (aunque ya las hicimos). Para estar seguros, puedes ejecutar:

```bash
python manage.py makemigrations
python manage.py migrate
```
Si no hay cambios en los modelos, `makemigrations` dirá "No changes detected". `migrate` aplicará las migraciones pendientes si las hay.

---

### **27 (reiterado). Por lo pronto solo trabajar con “Venta” dejar pendiente # MODELO: Cliente y # MODELO: Producto**

Ya hemos configurado las vistas y URLs para "Venta". Los modelos `Cliente` y `Producto` están registrados en `admin.py` para poder crear instancias manualmente desde el panel de administración de Django, lo cual es necesario para poder asociarlos a las Ventas. Las operaciones CRUD completas para Cliente y Producto se dejarán pendientes como se indicó.

---

### **28. Utilizar colores suaves, atractivos y modernos, el código de las páginas web sencillas.**

Se ha utilizado Bootstrap 5 con una paleta de colores basada en grises (`bg-dark`, `navbar-custom`) y colores de estado (azul para "ver", verde para "agregar", amarillo para "actualizar", rojo para "borrar"). El código HTML es sencillo y sigue la estructura de Bootstrap.

---

### **28 (reiterado). No validar entrada de datos.**

Confirmado. La validación se deja a la base de datos de Django para los campos `required` y la longitud máxima, pero no hay validaciones de formularios personalizadas en las vistas.

---

### **29. Al inicio crear la estructura completa de carpetas y archivos.**

La estructura de carpetas y archivos se ha creado y se ha guiado en los pasos anteriores. Aquí un resumen de la estructura final esperada:

```
UIII_Delrio_0459/
├── .venv/
├── backend_Delrio/
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py  <-- Modificado
│   └── wsgi.py
├── app_Delrio/
│   ├── migrations/
│   ├── templates/
│   │   ├── Ventas/
│   │   │   ├── agregar_venta.html
│   │   │   ├── actualizar_venta.html
│   │   │   ├── borrar_venta.html
│   │   │   └── ver_venta.html
│   │   ├── base.html
│   │   ├── footer.html
│   │   ├── header.html (no usado directamente en base.html, contenido fusionado)
│   │   ├── inicio.html
│   │   └── navbar.html
│   ├── __init__.py
│   ├── admin.py  <-- Modificado
│   ├── apps.py
│   ├── models.py <-- Modificado
│   ├── tests.py
│   ├── urls.py   <-- Creado
│   └── views.py  <-- Modificado
└── manage.py
```

---

### **30. Proyecto totalmente funcional.**

El proyecto está configurado para ser funcional en lo que respecta a las operaciones CRUD para el modelo `Venta`. Antes de probar las vistas de `Venta`, asegúrate de:

1.  **Crear un superusuario:**
    ```bash
    python manage.py createsuperuser
    ```
    Sigue las instrucciones para crear un usuario y contraseña.
2.  **Acceder al panel de administración:**
    Inicia el servidor: `python manage.py runserver 8459`
    Ve a `http://127.0.0.1:8459/admin/` y loguéate con el superusuario.
3.  **Crear al menos un Cliente y un Producto:**
    Desde el panel de administración, ve a `App Delrio` -> `Clientes` y crea uno.
    Luego ve a `App Delrio` -> `Productos` y crea uno. Esto es crucial para que puedas agregar ventas, ya que requieren un cliente y un producto existentes.

---

### **31. Finalmente ejecutar servidor en el puerto puerto 8459.**

```bash
python manage.py runserver 8459
```
Ahora, abre tu navegador y ve a `http://127.0.0.1:8459/`. Podrás ver la página de inicio, navegar a "Venta" en el menú, y empezar a agregar, ver y actualizar ventas (después de haber creado Clientes y Productos en el admin).
