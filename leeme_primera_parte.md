🧩 1. Crear carpeta del proyecto
mkdir UIII_Eventos_0301
cd UIII_Eventos_0301

💻 2. Abrir VS Code sobre la carpeta
code .

🧮 3. Abrir terminal en VS Code

Menú → Ver → Terminal

O presionar: Ctrl + ñ (Windows) o Ctrl + ` (Mac/Linux)

🐍 4. Crear entorno virtual .venv
python -m venv .venv

▶️ 5. Activar entorno virtual
En Windows:
.venv\Scripts\activate

En macOS/Linux:
source .venv/bin/activate

⚙️ 6. Activar intérprete de Python en VS Code

Ctrl + Shift + P → escribir "Python: Select Interpreter"

Seleccionar el que diga .venv

📦 7. Instalar Django
pip install django

🏗️ 8. Crear proyecto Django sin duplicar carpeta
django-admin startproject backend_eventos .

🚀 9. Ejecutar servidor en el puerto 8026
python manage.py runserver 8026

🌐 10. Copiar y pegar el link en el navegador
http://127.0.0.1:8026/

🧱 11. Crear aplicación app_eventos
python manage.py startapp app_eventos

🧬 12. Modelo models.py

(Ya lo tienes bien definido, lo mantenemos igual.)

⚙️ 12.5 Realizar migraciones
python manage.py makemigrations
python manage.py migrate

🧭 13–14. Trabajar con modelo Servicio y crear funciones en views.py

Archivo: app_eventos/views.py

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

🧩 15–22. Crear estructura de carpetas y archivos HTML
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


Cada HTML usa colores suaves, Bootstrap y estructura moderna.

🌍 24. Crear archivo urls.py en app_eventos
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

⚙️ 25. Agregar app_eventos en settings.py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app_eventos',
]

🌐 26. Configurar backend_eventos/urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_eventos.urls')),
]

🗃️ 27. Registrar modelos en admin.py
from django.contrib import admin
from .models import Servicio, Evento, Empleado

admin.site.register(Servicio)
admin.site.register(Evento)
admin.site.register(Empleado)


Luego, volver a ejecutar:

python manage.py makemigrations
python manage.py migrate

🎨 28–30. Colores suaves y diseño limpio

Usar Bootstrap 5, fondos claros, y fuentes elegantes.

🚀 31. Ejecutar servidor en puerto 8026
python manage.py runserver 8026


Abrir en el navegador:

http://127.0.0.1:8026/
