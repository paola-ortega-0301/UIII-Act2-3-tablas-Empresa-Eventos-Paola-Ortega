🧩 GUÍA DE CREACIÓN DEL PROYECTO DJANGO: UIII_Eventos_0301
🚀 1. Crear y abrir el proyecto
# Crear carpeta del proyecto
mkdir UIII_Eventos_0301
cd UIII_Eventos_0301


💻 Abrir Visual Studio Code

code .

🧮 2. Abrir la terminal en VS Code

Menú → Ver → Terminal

O usa el atajo:

Windows: Ctrl + ñ

Mac/Linux: `Ctrl + ``

🐍 3. Crear y activar entorno virtual
# Crear entorno virtual
python -m venv .venv


Activar entorno virtual:

💻 En Windows:

.venv\Scripts\activate


🍏 En macOS/Linux:

source .venv/bin/activate

⚙️ 4. Configurar el intérprete de Python en VS Code

Presiona Ctrl + Shift + P

Escribe: Python: Select Interpreter

Selecciona el que diga .venv

📦 5. Instalar Django
pip install django

🏗️ 6. Crear el proyecto principal
django-admin startproject backend_eventos .


(El punto evita que se duplique la carpeta.)

🌍 7. Ejecutar el servidor
python manage.py runserver 8026


🌐 Abre en tu navegador:
👉 http://127.0.0.1:8026/

🧱 8. Crear la aplicación
python manage.py startapp app_eventos

🧬 9. Crear el modelo en models.py

Tu modelo Servicio (y otros, si los tienes) ya están definidos.
Luego, realiza las migraciones:

python manage.py makemigrations
python manage.py migrate

🧭 10. Crear las vistas (views.py)

Ruta: app_eventos/views.py

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

🧩 11. Crear estructura de plantillas HTML

Ruta:

app_eventos/
│
├── templates/
│   ├── base.html
│   ├── header.html
│   ├── navbar.html
│   ├── footer.html
│   ├── inicio.html
│   └── servicio/
│       ├── agregar_servicio.html
│       ├── ver_servicio.html
│       ├── actualizar_servicio.html
│       └── borrar_servicio.html


🎨 Usa Bootstrap 5, fondos suaves, tipografía moderna y una interfaz clara.

🌐 12. Configurar las rutas (urls)

📁 Archivo: app_eventos/urls.py

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

⚙️ 13. Registrar la app en settings.py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app_eventos',
]

🌎 14. Configurar backend_eventos/urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_eventos.urls')),
]

🗃️ 15. Registrar los modelos en admin.py
from django.contrib import admin
from .models import Servicio, Evento, Empleado

admin.site.register(Servicio)
admin.site.register(Evento)
admin.site.register(Empleado)


Luego, ejecuta nuevamente:

python manage.py makemigrations
python manage.py migrate

🎨 16. Estilo y diseño

Utiliza Bootstrap 5 con colores suaves y un estilo limpio.
💡 Puedes basarte en la paleta:

Fondo: #f8f9fa

Acentos: #6c757d, #0d6efd

Fuentes modernas (ej. Poppins, Inter)

🚀 17. Ejecutar el servidor final
python manage.py runserver 8026


Abre tu navegador en:
👉 http://127.0.0.1:8026/
