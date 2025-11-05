Perfecto 👌 Aquí tienes tu guía **completa, detallada y estructurada exactamente igual** al formato del proyecto anterior (**Mueblería**), pero ahora adaptada para tu nuevo proyecto:

---

# 🎉 PROYECTO: EMPRESA DE EVENTOS

**Lenguaje:** Python
**Framework:** Django
**Editor:** Visual Studio Code

---

## 🧩 PRIMERA PARTE — PROCEDIMIENTO COMPLETO

---

### 1️⃣ Crear la carpeta del proyecto

1. Abre el **Explorador de archivos**.
2. Crea una nueva carpeta con el nombre:

   ```
   UIII_Eventos_0301
   ```
3. Guarda la carpeta en un lugar fácil de encontrar (por ejemplo, en **Documentos** o **Escritorio**).

---

### 2️⃣ Abrir VS Code sobre la carpeta

1. Abre **Visual Studio Code**.
2. En la barra superior selecciona: **Archivo → Abrir carpeta**.
3. Busca y selecciona la carpeta:

   ```
   UIII_Eventos_0301
   ```
4. Presiona **Seleccionar carpeta**.

---

### 3️⃣ Abrir la terminal en VS Code

En la barra superior de VS Code selecciona:
**Terminal → Nueva terminal**
Se abrirá una terminal en la parte inferior.

---

### 4️⃣ Crear el entorno virtual `.venv` desde la terminal

Ejecuta el siguiente comando:

```
python -m venv .venv
```

Esto creará una carpeta llamada **.venv** dentro del proyecto.

---

### 5️⃣ Activar el entorno virtual

En **Windows**, escribe:

```
.venv\Scripts\activate
```

Si se activa correctamente, verás **(.venv)** al inicio de la línea de comandos.

---

### 6️⃣ Activar el intérprete de Python en VS Code

1. Presiona **Ctrl + Shift + P**.
2. Escribe: **Python: Select Interpreter**.
3. Elige el intérprete:

   ```
   .venv\Scripts\python.exe
   ```

---

### 7️⃣ Instalar Django

Ejecuta el comando:

```
pip install django
```

Verifica la instalación:

```
django-admin --version
```

---

### 8️⃣ Crear el proyecto principal `backend_eventos`

En la terminal escribe:

```
django-admin startproject backend_eventos .
```

> El punto **"."** evita que se cree una carpeta duplicada.

---

### 9️⃣ Ejecutar el servidor en el puerto 8026

```
python manage.py runserver 8026
```

---

### 🔟 Copiar el enlace y abrir en el navegador

Copia y pega en el navegador:

```
http://127.0.0.1:8026/
```

Deberás ver la página de inicio de Django.

---

### 1️⃣1️⃣ Crear la aplicación `app_eventos`

```
python manage.py startapp app_eventos
```

---

### 1️⃣2️⃣ Crear el modelo **models.py**

Abre el archivo `app_eventos/models.py` y reemplaza su contenido con el siguiente código:

```python
from django.db import models

# ==========================================
# MODELO: SERVICIO
# ==========================================
class Servicio(models.Model):
    nombre = models.CharField(max_length=100)
    descripcion = models.TextField()
    costo = models.DecimalField(max_digits=10, decimal_places=2)
    duracion_horas = models.PositiveIntegerField()
    tipo = models.CharField(max_length=50)
    disponible = models.BooleanField(default=True)
    fecha_creacion = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.nombre


# ==========================================
# MODELO: EVENTO
# ==========================================
class Evento(models.Model):
    nombre = models.CharField(max_length=150)
    descripcion = models.TextField()
    fecha_inicio = models.DateField()
    fecha_fin = models.DateField()
    ubicacion = models.CharField(max_length=200)
    capacidad = models.PositiveIntegerField()
    precio_base = models.DecimalField(max_digits=10, decimal_places=2)
    servicios = models.ManyToManyField(Servicio, related_name='eventos')

    def __str__(self):
        return self.nombre


# ==========================================
# MODELO: EMPLEADO
# ==========================================
class Empleado(models.Model):
    nombre = models.CharField(max_length=100)
    apellido = models.CharField(max_length=100)
    puesto = models.CharField(max_length=100)
    salario = models.DecimalField(max_digits=10, decimal_places=2)
    telefono = models.CharField(max_length=20)
    email = models.EmailField(unique=True)
    fecha_contratacion = models.DateField()
    evento = models.ForeignKey(Evento, on_delete=models.CASCADE, related_name='empleados')

    def __str__(self):
        return f"{self.nombre} {self.apellido}"
```

---

### 1️⃣2️⃣.5️⃣ Realizar las migraciones

Ejecuta:

```
python manage.py makemigrations
python manage.py migrate
```

---

### 1️⃣3️⃣ Comenzar con el modelo **Servicio**

(Será el primero en usarse para el CRUD).

---

### 1️⃣4️⃣ Crear las funciones en **views.py**

Abre `app_eventos/views.py` y agrega las siguientes vistas:

```python
from django.shortcuts import render, redirect
from .models import Servicio

# Página de inicio
def inicio_evento(request):
    return render(request, 'inicio.html')

# Agregar servicio
def agregar_servicio(request):
    if request.method == 'POST':
        nombre = request.POST['nombre']
        descripcion = request.POST['descripcion']
        costo = request.POST['costo']
        duracion_horas = request.POST['duracion_horas']
        tipo = request.POST['tipo']
        disponible = request.POST.get('disponible', False) == 'on'

        nuevo = Servicio(
            nombre=nombre,
            descripcion=descripcion,
            costo=costo,
            duracion_horas=duracion_horas,
            tipo=tipo,
            disponible=disponible
        )
        nuevo.save()
        return redirect('ver_servicio')
    return render(request, 'servicio/agregar_servicio.html')


# Ver servicios
def ver_servicio(request):
    servicios = Servicio.objects.all()
    return render(request, 'servicio/ver_servicio.html', {'servicios': servicios})


# Actualizar servicio
def actualizar_servicio(request, id):
    servicio = Servicio.objects.get(id=id)
    return render(request, 'servicio/actualizar_servicio.html', {'servicio': servicio})


# Realizar actualización
def realizar_actualizacion_servicio(request, id):
    servicio = Servicio.objects.get(id=id)
    if request.method == 'POST':
        servicio.nombre = request.POST['nombre']
        servicio.descripcion = request.POST['descripcion']
        servicio.costo = request.POST['costo']
        servicio.duracion_horas = request.POST['duracion_horas']
        servicio.tipo = request.POST['tipo']
        servicio.disponible = request.POST.get('disponible', False) == 'on'
        servicio.save()
        return redirect('ver_servicio')
    return redirect('ver_servicio')


# Borrar servicio
def borrar_servicio(request, id):
    servicio = Servicio.objects.get(id=id)
    servicio.delete()
    return redirect('ver_servicio')
```

---

### 1️⃣5️⃣ Crear la carpeta **templates**

Ruta:

```
app_eventos/templates/
```

---

### 1️⃣6️⃣ Crear los archivos base del diseño

Dentro de **templates**, crea los siguientes archivos:

* base.html
* header.html
* navbar.html
* footer.html
* inicio.html

---

### 1️⃣7️⃣ Archivo **base.html**

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Empresa de Eventos</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body class="bg-light">
    {% include 'header.html' %}
    {% include 'navbar.html' %}
    <main class="container mt-4">
        {% block content %}{% endblock %}
    </main>
    {% include 'footer.html' %}
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```

---

### 1️⃣8️⃣ Archivo **navbar.html**

```html
<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">🎊 Sistema de Administración Eventos</a>
    <div class="collapse navbar-collapse">
      <ul class="navbar-nav ms-auto">
        <li class="nav-item"><a class="nav-link" href="/">🏠 Inicio</a></li>

        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" data-bs-toggle="dropdown">💼 Servicio</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="/agregar_servicio/">Agregar Servicio</a></li>
            <li><a class="dropdown-item" href="/ver_servicio/">Ver Servicio</a></li>
          </ul>
        </li>

        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" data-bs-toggle="dropdown">🎟️ Evento</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Agregar Evento</a></li>
          </ul>
        </li>

        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" data-bs-toggle="dropdown">👩‍💼 Empleado</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Agregar Empleado</a></li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</nav>
```

---

### 1️⃣9️⃣ Archivo **footer.html**

```html
<footer class="bg-dark text-white text-center py-3 fixed-bottom">
  <p>© <span id="anio"></span> Creado por Paola Ortega, CBTis 128</p>
</footer>
<script>
  document.getElementById("anio").innerText = new Date().getFullYear();
</script>
```

---

### 2️⃣0️⃣ Archivo **inicio.html**

```html
{% extends 'base.html' %}
{% block content %}
<div class="text-center">
    <h1 class="mb-3">Bienvenido al Sistema de Administración de Eventos</h1>
    <img src="https://upload.wikimedia.org/wikipedia/commons/3/3f/Event_management.jpg" width="400">
</div>
{% endblock %}
```

---

### 2️⃣1️⃣ Crear subcarpeta **servicio**

Ruta:

```
app_eventos/templates/servicio/
```

---

### 2️⃣2️⃣ Archivos HTML del CRUD de servicio

**agregar_servicio.html**

```html
{% extends 'base.html' %}
{% block content %}
<h2>Agregar Servicio</h2>
<form method="POST">
  {% csrf_token %}
  <input type="text" name="nombre" placeholder="Nombre" class="form-control mb-2">
  <textarea name="descripcion" placeholder="Descripción" class="form-control mb-2"></textarea>
  <input type="number" name="costo" placeholder="Costo" class="form-control mb-2">
  <input type="number" name="duracion_horas" placeholder="Duración (horas)" class="form-control mb-2">
  <input type="text" name="tipo" placeholder="Tipo" class="form-control mb-2">
  <div class="form-check mb-2">
    <input class="form-check-input" type="checkbox" name="disponible" id="disponible">
    <label class="form-check-label" for="disponible">Disponible</label>
  </div>
  <button type="submit" class="btn btn-success">Guardar</button>
</form>
{% endblock %}
```

**ver_servicio.html**

```html
{% extends 'base.html' %}
{% block content %}
<h2>Lista de Servicios</h2>
<table class="table table-bordered">
  <thead class="table-dark">
    <tr>
      <th>ID</th><th>Nombre</th><th>Tipo</th><th>Costo</th><th>Disponible</th><th>Acciones</th>
    </tr>
  </thead>
  <tbody>
    {% for s in servicios %}
    <tr>
      <td>{{ s.id }}</td>
      <td>{{ s.nombre }}</td>
      <td>{{ s.tipo }}</td>
      <td>{{ s.costo }}</td>
      <td>{% if s.disponible %}Sí{% else %}No{% endif %}</td>
      <td>
        <a href="/actualizar_servicio/{{ s.id }}/" class="btn btn-warning btn-sm">Editar</a>
        <a href="/borrar_servicio/{{ s.id }}/" class="btn btn-danger btn-sm">Borrar</a>
      </td>
    </tr>
    {% endfor %}
  </tbody>
</table>
{% endblock %}
```

**actualizar_servicio.html**

```html
{% extends 'base.html' %}
{% block content %}
<h2>Actualizar Servicio</h2>
<form method="POST" action="/realizar_actualizacion_servicio/{{ servicio.id }}/">
  {% csrf_token %}
  <input type="text" name="nombre" value="{{ servicio.nombre }}" class="form-control mb-2">
  <textarea name="descripcion" class="form-control mb-2">{{ servicio.descripcion }}</textarea>
  <input type="number" name="costo" value="{{ servicio.costo }}" class="form-control mb-2">
  <input type="number" name="duracion_horas" value="{{ servicio.duracion_horas }}" class="form-control mb-2">
  <input type="text" name="tipo" value="{{ servicio.tipo }}" class="form-control mb-2">
  <div class="form-check mb-2">
    <input class="form-check-input" type="checkbox" name="disponible" {% if servicio.disponible %}checked{% endif %}>
    <label class="form-check-label">Disponible</label>
  </div>
  <button type="submit" class="btn btn-primary">Actualizar</button>
</form>
{% endblock %}
```

---

### 2️⃣4️⃣ Crear archivo **urls.py** en `app_eventos`

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.inicio_evento, name='inicio_evento'),
    path('agregar_servicio/', views.agregar_servicio, name='agregar_servicio'),
    path('ver_servicio/', views.ver_servicio, name='ver_servicio'),
    path('actualizar_servicio/<int:id>/', views.actualizar_servicio, name='actualizar_servicio'),
    path('realizar_actualizacion_servicio/<int:id>/', views.realizar_actualizacion_servicio, name='realizar_actualizacion_servicio'),
    path('borrar_servicio/<int:id>/', views.borrar_servicio, name='borrar_servicio'),
]
```

---

### 2️⃣5️⃣ Agregar la app en **settings.py**

Abre `backend_eventos/settings.py` y agrega:

```python
INSTALLED_APPS = [
    ...,
    'app_eventos',
]
```

---

### 2️⃣6️⃣ Enlazar **urls.py** del proyecto

Abre `backend_eventos/urls.py` y modifica:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_eventos.urls')),
]
```

---

### 2️⃣7️⃣ Registrar modelos en **admin.py**

Abre `app_eventos/admin.py`:

```python
from django.contrib import admin
from .models import Servicio, Evento, Empleado

admin.site.register(Servicio)
admin.site.register(Evento)
admin.site.register(Empleado)
```

---

### 2️⃣8️⃣ Estilo

Usa colores suaves, modernos y atractivos (Bootstrap ya lo incluye).

---

### 2️⃣9️⃣ Estructura completa del proyecto

```
UIII_Eventos_0301/
│
├── .venv/
│
├── backend_eventos/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   ├── asgi.py
│   └── wsgi.py
│
├── app_eventos/
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── urls.py
│   ├── views.py
│   ├── migrations/
│   │    └── __init__.py
│   └── templates/
│        ├── base.html
│        ├── header.html
│        ├── navbar.html
│        ├── footer.html
│        ├── inicio.html
│        └── servicio/
│             ├── agregar_servicio.html
│             ├── ver_servicio.html
│             ├── actualizar_servicio.html
│             └── borrar_servicio.html
│
├── db.sqlite3
└── manage.py
```

---

### 3️⃣0️⃣ Ejecutar el servidor

```
python manage.py runserver 8026
```

Y abre en el navegador:

```
http://127.0.0.1:8026/
```

---

✅ **Proyecto completamente funcional, estructurado y listo para usar en VS Code.**
Los modelos **Evento** y **Empleado** se dejan pendientes para futuras fases.
