QUÉ ES AWS???

Argentina Was Srobbed o quizás también conocido por algunos pocos como Amazon Web Services, es el entorno cloud de Amazon (sisi, el que te vende los peluchitos y toda esa boludece que compra vo) para que puedas subir tus aplicaciones y servicios a la nube sin tener que preocuparte por el hardware, la infraestructura o el mantenimiento de servidores físicos. (mentira, te tenés que preocupar igual)

- QUÉ VENTAJAS TIENE?

Te pesa menos la billetera porque no vas a tener plata.

Chiste, como dije antes, no te tenés que preocupar mucho y es autoescalable, además si lo configuras bien pagás solo lo que usás. Y necesitás saberlo para los laburos, no hay mayor ventaja que esa.

Sobre todo el tema de la disponibilidad, al ser una estructura global de servidores tan grande, tiene mucha capacidad y estabilidad, nunca se cae!

r/devsarg - AWS outage
AWS outage
Nunca se cae dije.

- CÓMO SE USA??

Bueno, vamos a eso máquina.

Para empezar necesitamos saber dónde nos estamos metiendo, algo que tus viejos no pensaron cuando te hicieron, viste cómo e.

Los servicios más conocidos que ofrece AWS, junto a sus equivalentes de terceros son:

Servicio Utilidad Tecnologías Similares
EC2 Máquina virtual / VPS para ejecutar aplicaciones a nivel SO Hetzner, Linode, DigitalOcean, Hostinger
ECR Registro/repositorio de imágenes de contenedores Docker Hub, GHCR
ECS Cluster de orquestación de contenedores (usa EC2 internamente) Kubernetes, Docker Swarm
Fargate Contenedores serverless efímeros en ECS Lambda, Cloud Run
Lambda Ejecución serverless (son temporales y no tienen estado) Google Cloud Functions, Azure Functions, Vercel
DynamoDB Base de datos NoSQL MongoDB, Cassandra, Firebase Realtime
RDS Base de datos SQL relacional MySQL, PostgreSQL, MariaDB
API Gateway Enrutador de API con rate limit, autenticación, CORS, etc Traefik, Nginx, Kong API Gateway
Cognito Autenticación e identidad de usuarios Auth0, Okta, Firebase Auth
CloudWatch Monitoreo, registro y recopilación de métricas Prometheus, Grafana, ELK Stack, Datadog
SES Servicio de correo electrónico (envío SMTP) SendGrid, Mailgun
SQS Manejo de colas (jeje colas) para peticiones asincrónicas RabbitMQ, Kafka, Apache ActiveMQ
SNS Mensajería (SMS, notificaciones push) Kafka, NATS, Twilio
CloudFront CDN para almacenamiento en caché y servir archivos Cloudflare, Fastly, Bunny CDN
S3 Almacenamiento de objetos para archivos de todo tipo Google Cloud Storage, Azure Blob, Backblaze B2, Minio
IAM Gestión de Identidad y Acceso (roles, políticas, permisos) Keycloak, HashiCorp Vault
Y vos dirás, hermano eso es una banda no puede ser

No no tranqui, hay SESENTA MÁS. Para secretos, cache tipo Redis, Route53 para DNS, VPC para redes LAN dentro de tu cuenta, etc etc y muuchas etc.

Pero bueno esos por encima son los más usados y pedidos. Los necesarios para construir casi cualquier proyecto con lo esencial para empezar.

ACLARACIÓN: Qué es esto de serverless? Sin estado? Lambdas?

Cuando vos usas estos servicios lo que está pasando es que aws levanta una mini máquina virtual por así decirlo que corre el código que vos le diste, en vez de dejar una api/servidor corriendo 24/7, solo corre cuando se la llama, y al terminar la ejecución se apaga automáticamente. Esto tiene sus ventajas y sus desventajas. Lo bueno es que pagás solo lo que usás, y es autoescalable, si hay 2 peticiones (un ejemplo nomás) levanta 2 lambdas, si hay 200 mil, levanta las necesarias. Lo malo? Hay que darle unos milisegundos de tiempo para que arranque, ya que está levantando todo de 0, pero bueno, hay cosas más avanzadas para recortar ese tiempo de arranque, pero eso es para otro post.

(Y no te preocupes, algunos piden/usan GCP o Azure, que son plataformas cloud de Google y Microsoft, los conceptos en sí son parecidos, no es que es otro mundo a parte, pero AWS es el más usado hoy en día y todos tienen esos nombrecitos que no sabés si es una app o la hija de Elon Musk)

Dependiendo de qué estés queriendo hacer a veces serverless puede ser conveniente y bueno otras veces no.

Ahora te estarás preguntando, joya, pero, cuánto vale? Cómo lo pruebo? Cómo gano plata DIOS AYÚDAME

Ok ok, te contesto las que puedo, cuánto vale ${PBT}porque estaban por cambiar algunos planes creo estos meses y ya le perdí el rastro, pero la mayoría de servicios tienen planes gratuitos generosos y además Amazon te regala créditos apenas te creas la cuenta por unos meses. Lo de como hacer plata si encontrás avisame.

PERO, hojaldre acá, tenemos una opción muy copada para probar y jugar con todo esto en local simulando un entorno de AWS real con Floci.

Floci es un servicio que corren en contenedores Docker muy fácil de instalar y de usar, lo único que hay que hacer es instalarlo y darle la ruta a lo que sería AWS real para mockearlo. Mira es re fácil:

Armamos un docker-compose.yml con:

services:
floci:
image: floci/floci:latest
container_name: floci
ports: - "127.0.0.1:4566:4566" # Para que corra solo en local por las dudas y las blandas también
volumes: - /var/run/docker.sock:/var/run/docker.sock # Floci necesita poder crear/bajar contenedores por su cuenta - floci-data:/var/lib/floci

volumes:
floci-data:
También se puede instalar como servicio local pero te recomiendo hacerlo así en Docker (en Linux) para evitarte problemas y que no te queden cosas dando vueltas después. Cualquier cosa mirate cómo se instala en otros sistemas o a nivel host: https://floci.io/#install

Ahora solo tenemos que exportar las variables de los url a localhost para no comunicarnos con AWS real si no con Floci. En nuestro .bashrc/.zshrc al final agregamos:

export AWS_ENDPOINT_URL=http://localhost:4566
export AWS_DEFAULT_REGION=us-east-1
export AWS_ACCESS_KEY_ID=test
export AWS_SECRET_ACCESS_KEY=test
Ahora lo levantamos con docker compose up -d y ya ta, descarga la imagen y tuki sos Jeff Bezos.

Cómo le hacemo para conectar ahí entonce??

Simple, con la cli oficial de AWS.

- Qué es la cli de AWS?

Es un set de herramientas de AWS para utilizar su ecosistema pero desde la consola, sin pasar por los dashboards de la página web. En este caso eso último sería imposible porque no estamos usando AWS per se si no una simulación local con Floci. Y además siempre mejor aprender la cli porque es lo que más usarías en un entorno real.

Vamos a instalarla:

https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

En Linux:

curl -fsSL https://awscli.amazonaws.com/v2/install.sh | bash
Termina todo eso y ya con el comando aws --version deberías ver algo como aws-cli/2.13.9 Python/3.11.4 Linux/6.5.0-1009-azure exe/x86_64 prompt/off

Qué es lo que está pasando por atrás de todo esto para que AWS use Floci y no AWS oficial entonces? Bueno, esas variables que agregamos el .bashrc o a donde sea que lo hayas agregado, lo que están haciendo es interceptar las peticiones de AWS que irían al datacenter real y decirle "che, vos estás acá en realidad (en el localhost:puerto)"

Con esto podemos simular el mismo entorno que usarías en producción sin gastar un solo centavo y ni siquiera crear una cuenta. Obviamente existen los free tier de algunos servicios y créditos al crear la cuenta por primera vez pero, estos créditos vencen y no son infinitos. Aprendiendo en local luego podemos pasar al entorno real con las mismas herramientas, replicar los mismos servicios y ahí sí hacer uso de los créditos o hasta pagar por el servicio si es que lo necesitamos.

Y qué tal Floci? Qué está haciendo por detrás? Un picarón el Floci.

Lo que hace este servicio es, con el binario de la app, levantar mini contenedores a discreción cuando lo necesite según lo que estés pidiéndole, después de todo es más o menos algo parecido a lo que hace AWS. En el servicio oficial todos esos nombrecitos que tienen tan complicados no son más que mini apps que se comunican entre sí mediante apis internas en un ecosistema cerrado. Bastante modular todo.

Cómo probamos que todo quedó bien entonces?

Bueno podemos probar algún comando bien básico por ejemplo con S3, subiendo un archivo y recuperándolo después. Primero creamos un bucket (que es como una carpeta gigante en la nube) en consola:

# Creamos el bucket.

aws s3 mb s3://my-bucket

# Escribimos algo a un archivo de texto.

echo "Jeff dame laburo." > practica-floci.txt

# Copiamos el archivo al bucket.

aws s3 cp practica-floci.txt s3://my-bucket/practica-floci.txt

# Copiamos el archivo de vuelta a local en otro archivo.

aws s3 cp s3://my-bucket/practica-floci.txt the-game.txt

cat the-game.txt
Con estas herramientas podés empezar a jugar y probar mil millones de cosas, te recomiendo empezar con alguna lambda, que por ejemplo te guarde cosas en un S3 y en una tabla de DynamoDB, para que vayas probando cómo se usa el ecosistema serverless y el almacenamiento. Si ya estás más canchero cosas como ECS y Fargate para levantar contenedores y hacer microservicios, o incluso un API Gateway con Cognito para autenticar usuarios y que puedan subir archivos a tu S3. También IAM para manejo de permisos y roles entre servicios y usuarios, muy útil.

No entendiste un carajo pero igual te interesa y querés aprender o te pasa que lo ves en TODOS los puestos que publican pero te suena a chino y te da miedo?

No seas pMandale a tu llm de turno, el que más te guste! preguntale para que te explique cómo usar más o menos los servicios andá probando cosas, recordá que estás en local así que no hay mucho para romper, más si Floci está corriendo en Docker, incluso podés jugar a levantar el llm/aws-cli en otro contenedor así no toca nada raro del host y que se comunique en la misma red de Docker que Floci y así tenés todo 100% aislado y seguro. No te preocupes no es tan complicado. ${PBT} y son 2 minutos. (una maratón)

También se le pueden agregar mil y un skills o MCPs para facilitar todo el flujo de trabajo con agentes y demás, pero te recomiendo empezar con lo manual de esta forma y más adelante incorporar lo fácil y automático. Además también existe sam que es otra cli oficial que trabaja en conjunto con AWS para automatizar todavía más y más cosas, muy útil cuando ya estás canchero/a. Si les interesa puedo hacer otro post más adelante con todas estas cosas y cómo se usan.

Si querés leer documentación (hacelo, no seas papanatas) buscate la oficial de AWS o te dejo una que suelo usar para cuando no quiero tanto texto por todos lados e ir a lo que me interesa seccionado: https://www.w3schools.com/aws/aws_cloudessentials_intro.php Todo gratis y tiene algunos ejercicios y evaluaciones simples para ver qué onda. Uso y recomiendo, no solo para AWS, chusmealo. También hay certificaciones oficiales de AWS y todo eso que le gusta a RRHH.
