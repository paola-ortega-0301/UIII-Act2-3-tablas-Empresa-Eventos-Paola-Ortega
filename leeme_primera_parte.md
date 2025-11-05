# 🎯 Práctica 3: Aplicación Web de Gestión de Eventos (Proyecto Completo y Funcional)

¡Bienvenido! En esta práctica crearás una aplicación web completa con Django para administrar servicios y eventos.
Podrás agregar, visualizar, actualizar y eliminar servicios, con un diseño limpio, colores suaves y una estructura profesional.

## 🧰 Tecnologías y Requisitos
Herramienta	Descripción
🐍 Python	3.8 o superior
🌐 Django	Framework web principal
💻 Editor	Visual Studio Code (recomendado)
💾 Base de Datos	SQLite3 (por defecto)
🎨 Diseño	Bootstrap 5, colores suaves
📁 Estructura Final del Proyecto
UIII_Eventos_0301/
├── .venv/                   # Entorno virtual
├── backend_eventos/         # Configuración del proyecto Django
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── app_eventos/             # Aplicación principal
│   ├── __init__.py
│   ├── admin.py
│   ├── models.py
│   ├── views.py
│   ├── urls.py
│   ├── templates/
│   │   ├── base.html
│   │   ├── header.html
│   │   ├── navbar.html
│   │   ├── footer.html
│   │   ├── inicio.html
│   │   └── servicio/
│   │       ├── agregar_servicio.html
│   │       ├── ver_servicio.html
│   │       ├── actualizar_servicio.html
│   │       └── borrar_servicio.html
│   └── static/
│       └── css/
│           └── styles.css
├── manage.py
└── requirements.txt

## ⚙️ Paso 1: Crear y Configurar el Proyecto
🧩 Crear carpeta y abrir en VS Code
mkdir UIII_Eventos_0301
cd UIII_Eventos_0301
code .

💻 Abrir la terminal en VS Code

Menú → Ver → Terminal

Atajo:

Windows: Ctrl + ñ

macOS/Linux: `Ctrl + ``

## 🐍 Paso 2: Crear y Activar el Entorno Virtual
python -m venv .venv


Activar entorno virtual:

💻 Windows:

.venv\Scripts\activate


🍏 macOS/Linux:

source .venv/bin/activate

## 📦 Paso 3: Instalar Django
pip install django

🏗️ Paso 4: Crear el Proyecto Principal
django-admin startproject backend_eventos .


⚠️ El punto (.) evita que se duplique la carpeta.

## 🌍 Paso 5: Ejecutar el Servidor
python manage.py runserver 8026


Abre tu navegador en 👉 http://127.0.0.1:8026/

Si ves la página de Django, ¡todo está funcionando! ✅

## 🧱 Paso 6: Crear la Aplicación
python manage.py startapp app_eventos


Agrega la app a INSTALLED_APPS en backend_eventos/settings.py:

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app_eventos',
]

## 🧬 Paso 7: Modelos y Migraciones

📄 Archivo: app_eventos/models.py
(Asegúrate de tener tu modelo Servicio definido correctamente.)

Aplica las migraciones:

python manage.py makemigrations
python manage.py migrate

## 🧭 Paso 8: Vistas (views.py)

📄 Archivo: app_eventos/views.py

from django.shortcuts import render, redirect, get_object_or_404
from .models import Servicio

def inicio_evento(request):
    return render(request, 'inicio.html')

def agregar_servicio(request):
    if request.method == 'POST':
        nombre = request.POST['nombre']
        descripcion = request.POST['descripcion']
        costo = request.POST['costo']
        duracion_horas = request.POST['duracion_horas']
        tipo = request.POST['tipo']
        Servicio.objects.create(
            nombre=nombre,
            descripcion=descripcion,
            costo=costo,
            duracion_horas=duracion_horas,
            tipo=tipo
        )
        return redirect('ver_servicio')
    return render(request, 'servicio/agregar_servicio.html')

def ver_servicio(request):
    servicios = Servicio.objects.all()
    return render(request, 'servicio/ver_servicio.html', {'servicios': servicios})

def actualizar_servicio(request, id):
    servicio = get_object_or_404(Servicio, id=id)
    return render(request, 'servicio/actualizar_servicio.html', {'servicio': servicio})

def realizar_actualizacion_servicio(request, id):
    servicio = get_object_or_404(Servicio, id=id)
    servicio.nombre = request.POST['nombre']
    servicio.descripcion = request.POST['descripcion']
    servicio.costo = request.POST['costo']
    servicio.duracion_horas = request.POST['duracion_horas']
    servicio.tipo = request.POST['tipo']
    servicio.save()
    return redirect('ver_servicio')

def borrar_servicio(request, id):
    servicio = get_object_or_404(Servicio, id=id)
    servicio.delete()
    return redirect('ver_servicio')

## 🌐 Paso 9: Rutas (urls.py)

📄 Archivo: app_eventos/urls.py

from django.urls import path
from . import views

urlpatterns = [
    path('', views.inicio_evento, name='inicio'),
    path('servicio/agregar/', views.agregar_servicio, name='agregar_servicio'),
    path('servicio/ver/', views.ver_servicio, name='ver_servicio'),
    path('servicio/actualizar/<int:id>/', views.actualizar_servicio, name='actualizar_servicio'),
    path('servicio/realizar_actualizacion/<int:id>/', views.realizar_actualizacion_servicio, name='realizar_actualizacion_servicio'),
    path('servicio/borrar/<int:id>/', views.borrar_servicio, name='borrar_servicio'),
]


📄 Archivo principal: backend_eventos/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_eventos.urls')),
]

## 🗃️ Paso 10: Registrar Modelos en el Panel de Administración

📄 Archivo: app_eventos/admin.py

from django.contrib import admin
from .models import Servicio, Evento, Empleado

admin.site.register(Servicio)
admin.site.register(Evento)
admin.site.register(Empleado)


Luego, ejecuta nuevamente:

python manage.py makemigrations
python manage.py migrate

## 🎨 Paso 11: Plantillas y Diseño

📁 Estructura:

app_eventos/
└── templates/
    ├── base.html
    ├── header.html
    ├── navbar.html
    ├── footer.html
    ├── inicio.html
    └── servicio/
        ├── agregar_servicio.html
        ├── ver_servicio.html
        ├── actualizar_servicio.html
        └── borrar_servicio.html


💡 Diseño sugerido:

Usa Bootstrap 5

Fondo: #f8f9fa

Colores acento: #6c757d, #0d6efd

Fuente: Poppins o Inter

## 🚀 Paso 12: Ejecutar el Servidor Final
python manage.py runserver 8026


🌐 Abre tu navegador:
👉 http://127.0.0.1:8026/
