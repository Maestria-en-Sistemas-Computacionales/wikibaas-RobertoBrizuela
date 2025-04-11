# **Integración del BaaS y Supabase con IoT**

## **¿Porqué el Internet de las Cosas (IoT) es tan importante?**
El Internet de las Cosas (IoT) se ha consolidado como una de las tecnologías más relevantes y sorprendentes de la actualidad. A través de dispositivos IoT, se establece una conexión con objetos cotidianos, abarcando desde vehículos y electrodomésticos hasta sistemas de vigilancia infantil y termostatos, todo ello mediante dispositivos integrados [3].

La red de internet ha facilitado una comunicación fluida entre procesos, individuos y objetos. Impulsado por la computación de bajo costo, el análisis de grandes volúmenes de datos, la computación en la nube y tecnologías móviles avanzadas, los objetos físicos tienen la capacidad de compartir y conectar información con una mínima intervención humana.

En el actual panorama hiperconectado, estos sistemas pueden supervisar, ajustar y registrar cada interacción con los elementos que forman parte de esta red.

   ![IoTBaaS](images/canvadesign.png)

## **Ventajas de Integrar BaaS con IoT**

❇️ **Reducción de costos de desarrollo**

Las soluciones BaaS ofrecen servicios gestionados (como servidores e infraestructura de aplicaciones), lo que permite ahorrar recursos y reducir los gastos asociados al desarrollo.

❇️ **Tiempo de lanzamiento más rápido**

Con BaaS, los desarrolladores de IoT pueden enfocarse en mejorar la experiencia del usuario, sin preocuparse por aspectos como la administración de bases de datos, escalabilidad u otras tareas técnicas que retrasan el lanzamiento.

❇️ **Sin complicaciones de gestión de servidores**

BaaS se encarga de la infraestructura y configuración de servidores, eliminando la necesidad de dedicar tiempo y esfuerzo a su mantenimiento. Esto libera a los equipos para priorizar el desarrollo de funcionalidades clave.

Supabase puede ser integrado con aplicaciones web que consumen datos de dispositivos IoT mediante sus APIs REST y Realtime.


❇️ **Lenguaje técnico simplificado:**

✴️ **Gestionado:** Servicios administrados por el proveedor (no por el equipo de desarrollo).

✴️ **Escalabilidad:** Capacidad de adaptar recursos según demanda sin esfuerzo manual.

✴️ **Infraestructura:** Entorno técnico (servidores, redes, almacenamiento) necesario para el funcionamiento de una aplicación.


## **Integración de Supabase con una aplicación web que consume datos de un dispositivo IoT**

Existen servicios y plataformas (por ejemplo, Pipedream) que facilitan la integración entre dispositivos IoT y Supabase. Un ejemplo práctico es la integración del Adafruit IO API con la API de Supabase, donde Pipedream actúa como intermediario para recibir datos desde el dispositivo IoT y posteriormente insertar o actualizar la base de datos de Supabase, permitiendo que la aplicación web consuma esos datos de forma inmediata [13].

 ![IoTBaaS](images/Pipedream-logo.png){ .center-image }

➡️ **Descripción general de Adafruit IO**

 ![IoTBaaS](images/adafruit.png){ .center-image }

Adafruit IO es una API diseñada para la creación de aplicaciones del Internet de las Cosas (IoT). Ofrece un medio para almacenar, compartir y gestionar datos provenientes de tus dispositivos IoT. Con Adafruit IO, puedes crear paneles de control para mostrar datos en tiempo real, activar eventos basados en datos e incluso controlar salidas. Es una plataforma versátil que es especialmente amigable para aquellos que se inician en el mundo del IoT.

➡️ **Conectar Adafruit IO**

    #Python

    import requests

    def handler(pd: "pipedream"):
    headers = {"X-AIO-Key": f'{pd.inputs["adafruit_io"]["$auth"]["active_key"]}'}
    r = requests.get('https://io.adafruit.com/api/v2/user', headers=headers)
    # Export the data for use in future steps
    return r.json()

➡️  **Conectar Supabase** 

        #Python
        
        import requests

        def handler(pd: "pipedream"):
        token = f'{pd.inputs["supabase"]["$auth"]["service_key"]}'
        authorization = f'Bearer {token}'
        headers = {"Authorization": authorization, "apikey": f'{pd.inputs["supabase"]["$auth"]["service_key"]}'}
        r = requests.get(f'https://{pd.inputs["supabase"]["$auth"]["subdomain"]}.supabase.co/rest/v1/', headers=headers)
        # Export the data for use in future steps
        return r.json()
