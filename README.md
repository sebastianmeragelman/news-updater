# news-updater
news to wp post from rss

El sistema consta de tres partes:
> Archivo json en drive para guardar los news
> Notebook para colab que ejecuta un scrapp de los rss indicados y los guarda en formato json en un archivo de drive
> Plugin de wordpress para importar las noticias del archivo json y publicarlas.

Paso 1 — Crear archivo en Drive
Entrás a Drive
Click derecho → “Nuevo archivo” (o subir JSON vacío)

Podés crear un archivo con contenido inicial:

[]

Paso 2 — Obtener el ID

Cuando abrís el archivo en Drive:

URL:

https://drive.google.com/file/d/1ABC123XYZ/view

👉 el ID es:

1ABC123XYZ



Paso 3 — Usarlo en tu pipeline
url_drive = "https://drive.google.com/file/d/1ABC123XYZ/view"







INSTALAR EL PLUGIN WP-CRON

PASO 2 — Conectar tu función al cron

Esto es lo importante:

add_action('json_importer_event', 'importar_desde_json');

👉 Esto hace que WordPress ejecute tu función automáticamente.

🔥 PASO 3 — Definir frecuencia

WordPress trae 3 por defecto:

- hourly
- twicedaily
- daily



Agregá esto:

add_filter('cron_schedules', function($schedules) {

    $schedules['every_15_minutes'] = [
        'interval' => 900,
        'display'  => 'Cada 15 minutos'
    ];

    return $schedules;
});




Se debe de configurar en el archivo importador-json.php  la seccion

// 🔧 CONFIG define('JSON_URL', 'https://drive.google.com/uc?export=download&id=');

Completando luego de &id=  con el ID del archivo json
Y cambiás:

wp_schedule_event(time(), 'every_15_minutes', 'json_importer_event');
