# Laboratorio 15 — CloudFormation y Docker en Amazon EC2

Desarrollo de Soluciones en la Nube — TECSUP.

## Objetivos

En este laboratorio implemento infraestructura como código con CloudFormation, automatizo Docker mediante UserData y verifico aplicaciones web en Amazon EC2. Practico la actualización declarativa, la parametrización y la limpieza de recursos.

## Caso de negocio

Resuelvo el caso de una startup que necesita desplegar aplicaciones Docker para pruebas de aceptación. Defino la infraestructura en YAML para poder recrearla y reducir la configuración manual.

## Arquitectura

El navegador accede por HTTP a la IPv4 pública de una instancia EC2 dentro de una subred pública existente. Un Internet Gateway proporciona conectividad; el Security Group permite TCP 80. Amazon Linux 2023 instala Docker y ejecuta un contenedor con el puerto 80 publicado. CloudFormation administra la instancia y el Security Group. Reutilizo la VPC y la subred; no creo bases de datos, balanceadores ni NAT Gateways.

## Tecnologías

Utilizo AWS CloudFormation, EC2, VPC, Security Groups, Systems Manager Parameter Store, Amazon Linux 2023 x86_64, Docker, Nginx, nginxdemos/hello, Apache httpd, YAML, Git, GitHub, AWS CLI y Visual Studio Code.

## Estructura

```text
LAB15-NUBEE/
├── ec2-docker.yaml
├── README.md
├── prompts.md
├── .gitignore
├── registro-evidencias.md
├── plantillas/
│   ├── 01-nginx.yaml
│   ├── 02-nginx-hello.yaml
│   ├── 03-tags.yaml
│   ├── 04-keypair.yaml
│   └── 05-httpd.yaml
└── evidencias/  # originales locales excluidos de Git
```

## Inicialización y control de versiones

Leí íntegramente la guía adjunta de siete páginas y conservé intacto el documento académico original. Reutilicé la carpeta previamente seleccionada `/Users/naomisanchez/Documents/ChatGPT/LAB15-NUBEE`, evitando crear otra carpeta con el mismo proyecto. Verifiqué Git y permisos de escritura de GitHub; `git ls-remote` confirmó el remoto vacío. Trabajé en `main`, con commits y pushes progresivos, sin force push.

## Resultados verificados

| Actividad | Resultado real |
|---|---|
| Paso 1: plantilla | Validación AWS satisfactoria |
| Paso 2: creación | Stack creado mediante AWS Console |
| Paso 3: recursos | CREATE_COMPLETE, EC2 running, Security Group y Outputs verificados |
| Paso 4: Nginx | HTTP 200 y Welcome to nginx! |
| Paso 5: Nginx Hello | UPDATE_COMPLETE, nueva instancia, HTTP 200 y Hello World |
| Desafío 1: tags | Name = Laboratorio-Docker, sin reemplazo |
| Desafío 2: clave | KeyName asociado a nueva EC2; HTTP 200 conservado |
| Desafío 3: Docker Hub | Apache httpd, UPDATE_COMPLETE, HTTP 200 e It works! |
| Limpieza | Pendiente de confirmación antes de eliminar |

## Paso 1 — Plantilla validada

He creado `ec2-docker.yaml` y su copia `plantillas/01-nginx.yaml`. AWS CloudFormation validó la plantilla en `us-east-1`. Resolví la AMI oficial AL2023 x86_64 mediante SSM y comprobé la subred pública y ruta al Internet Gateway. Uso Docker con `dnf`, IMDSv2, disco gp3 cifrado de 8 GiB eliminable y créditos de CPU estándar. Mantengo SSH cerrado mientras no se necesite; HTTP 80 es público. No publico valores reales de red o cuenta.

La cuenta no devolvió beneficios EC2 en la consulta Free Tier. El usuario autorizó hasta US$1. La consulta AWS Pricing confirmó US$0.0116/h para t2.micro, más IPv4 y EBS.

## Paso 2 — Creación del stack

He cargado la plantilla y creado `lab-cloudformation-docker` mediante AWS Console en `us-east-1`, con t2.micro y una VPC y subred públicas existentes. Verifiqué `CREATE_IN_PROGRESS` mediante consola y CLI. No he creado una VPC ni un NAT Gateway. Las capturas de carga, parámetros y creación están guardadas localmente.

## Paso 3 — Recursos verificados

He comprobado `CREATE_COMPLETE`, los dos recursos del stack, la instancia `running`, los Outputs y la regla HTTP 80. IMDSv2 requiere token. No existe entrada SSH. Los valores reales de cuenta, IP e identificadores se conservan solo en evidencias locales excluidas de Git.

## Paso 4 — Nginx verificado

He abierto la IP vigente en Chrome y comprobado “Welcome to nginx!”. `curl` devolvió `HTTP/1.1 200 OK`, con servidor Nginx. La consulta inicial no conectó mientras arrancaba UserData; la siguiente fue exitosa sin cambiar la infraestructura. No he accedido por SSH a esta instancia.

## Paso 5 — Actualización a Nginx Hello

He desplegado `nginxdemos/hello` mediante la sustitución controlada de `WebServer` por `WebServerHello`, conservando el stack y el Security Group. Revisé la vista previa Add/Remove en consola y comprobé `UPDATE_COMPLETE`. Volví a consultar Outputs; la IP cambió. `curl` devolvió HTTP 200 y Chrome mostró Hello World con los datos del contenedor. La instancia anterior fue retirada por CloudFormation.

## Desafío 1 — Etiquetas

He agregado `Name = Laboratorio-Docker` a la instancia y guardado `plantillas/03-tags.yaml`. La vista previa indicó Modify con sustitución False. Verifiqué `UPDATE_COMPLETE`, la etiqueta en EC2 y HTTP 200 de Hello sin reemplazar la instancia.

## Desafío 2 — Key Pair

He incorporado el parámetro `KeyName` y `KeyName: !Ref KeyName`, manteniendo etiquetas y Hello. Creé un par RSA exclusivo `lab15-docker-keypair` e importé únicamente su clave pública en AWS. La clave privada permanece fuera del repositorio, en una carpeta local con permisos 700 y archivo con permisos 600. Seleccioné el par en consola; CloudFormation reemplazó EC2, finalizó con `UPDATE_COMPLETE` y la instancia nueva muestra la asociación real. Consulté los nuevos Outputs y confirmé HTTP 200 con el título Hello World. SSH continúa cerrado.

## Desafío 3 — Apache desde Docker Hub

He desplegado la imagen oficial `httpd` y conservado la versión final en `plantillas/05-httpd.yaml` y `ec2-docker.yaml`. Revisé Add WebServerApache y Remove WebServerHello, manteniendo el Security Group, la etiqueta Name y el Key Pair. Verifiqué `UPDATE_COMPLETE`, los nuevos Outputs, la instancia running con etiquetas y clave, HTTP 200 con cabecera Apache y la página “It works!” en Chrome.

## Correcciones técnicas respecto de la guía

Sustituí la AMI fija por el parámetro oficial SSM de AL2023 x86_64. Reemplacé `yum` y `amazon-linux-extras` por `dnf install -y docker`, sin actualización completa del sistema. Corregí la sangría de UserData con `Fn::Base64: !Sub |` y activé Docker con systemd. Configuré reinicio automático del contenedor, IMDSv2 obligatorio y volumen raíz gp3 cifrado de 8 GiB con `DeleteOnTermination`.

Parametricé VPC y subred, asocié el Security Group mediante su ID y solicité expresamente IPv4 pública. Mantengo SSH cerrado porque las verificaciones no requieren acceso remoto. No expongo el puerto 22 a Internet. Para cada cambio de imagen utilizo otro identificador lógico de instancia y actualizo los Outputs, asegurando un arranque que ejecute el nuevo UserData. Uso créditos de CPU estándar para evitar cargos por CPU ilimitada.

## Reproducción

Requiero una cuenta con permisos para CloudFormation, EC2, Security Groups, consulta SSM y pares de claves. Selecciono una VPC existente y una subred pública de la misma VPC con ruta activa `0.0.0.0/0` al Internet Gateway. Compruebo disponibilidad y cuotas de una instancia micro x86_64 antes de crear recursos.

1. Abro la carpeta en VS Code y valido la plantilla con `aws cloudformation validate-template --template-body file://plantillas/01-nginx.yaml --region us-east-1`.
2. En AWS Console abro CloudFormation, creo `lab-cloudformation-docker`, cargo `01-nginx.yaml` e ingreso VPC y subred.
3. Espero `CREATE_COMPLETE`, reviso Resources y Outputs y accedo a la IP pública por HTTP después de que termine UserData.
4. Actualizo el mismo stack con las plantillas 02, 03, 04 y 05 en orden, revisando los cambios antes de enviarlos y esperando `UPDATE_COMPLETE` entre etapas.
5. Selecciono un Key Pair propio y autorizado para 04 y 05. Conservo su clave privada fuera de Git.
6. Vuelvo a consultar Outputs después de cada reemplazo; verifico la página y `curl` en la IP vigente.
7. Después de guardar evidencias y confirmar la eliminación, elimino exclusivamente el stack del laboratorio y verifico sus recursos.

| Parámetro | Uso |
|---|---|
| VpcId | VPC existente autorizada |
| SubnetId | Subred pública de esa VPC |
| InstanceType | t2.micro por defecto; t3.micro como alternativa validada |
| LatestAmiId | Ruta SSM oficial AL2023 x86_64 |
| KeyName | Nombre de un Key Pair existente, requerido desde la versión 04 |

## Costos y seguridad

Trabajé en `us-east-1` con `t2.micro`. No asumí gratuidad: la consulta de uso Free Tier no devolvió beneficios EC2; la consulta del plan confirmó un plan PAID activo con créditos. No publico su saldo ni garantizo que cubra cargos concretos. Consulté AWS Pricing y obtuve US$0.0116/h para Linux t2.micro; IPv4 pública tiene tarifa publicada de US$0.005/h, más EBS y transferencias que correspondan. El presupuesto autorizado es US$1 para esta ejecución. La estimación no sustituye la factura final; los beneficios y el consumo de otros proyectos no forman parte de esta verificación.

Los reemplazos implican un breve solapamiento de instancias. No configuro credenciales permanentes ni cambio permisos IAM. No publico identificadores de cuenta, IP personal, claves privadas ni los Outputs reales. Las evidencias originales se conservan localmente para mi entrega académica y no se publican en GitHub.

Fuentes de tarifas: [EC2 T2](https://aws.amazon.com/ec2/instance-types/t2/), [IPv4 pública](https://aws.amazon.com/vpc/pricing/).

## Conclusiones

1. Comprendí cómo CloudFormation convierte una plantilla YAML en recursos relacionados y verificables.
2. Aprendí a comprobar la compatibilidad de la AMI y los comandos de inicialización con Amazon Linux 2023.
3. Distinguí el estado de infraestructura de CloudFormation de la disponibilidad HTTP de la aplicación.
4. Apliqué el reemplazo declarativo de instancias para ejecutar un UserData nuevo durante el cambio de contenedor.
5. Reconocí la importancia de controlar costos, proteger las claves y verificar la eliminación de los recursos del laboratorio.

## Evidencias locales

Conservo las capturas originales localmente para mi entrega académica. Las imágenes de la herramienta fueron convertidas a PNG sin alterar su contenido; conservo también los bytes originales en la carpeta local ignorada `.local/capturas-originales/`. El registro público describe las capturas, pero no publica imágenes ni datos sensibles.

| Etapa | Carpeta dentro de evidencias | Capturas al cierre del despliegue |
|---|---|---:|
| Inicialización | 00-inicializacion | 3 |
| Plantilla | 01-plantilla | 2 |
| Creación | 02-creacion-stack | 3 |
| Recursos | 03-verificacion-recursos | 5 |
| Nginx | 04-despliegue-nginx | 2 |
| Hello | 05-actualizacion-stack | 4 |
| Tags | 06-desafio-tags | 3 |
| Key Pair | 07-desafio-keypair | 4 |
| Apache | 08-desafio-dockerhub | 4 |
| Limpieza | 09-limpieza-recursos | Pendiente |

## Limpieza de recursos

He comprobado que los cinco pasos, los tres desafíos, las 30 capturas y la publicación de las plantillas están completos. La eliminación del stack y del Key Pair exclusivo está pendiente de la decisión del usuario. La herramienta de control de Chrome perdió acceso a la ventana con el error `cgWindowNotFound`; solicité restaurarla para continuar la limpieza con evidencias de consola. AWS CLI sigue disponible. No he eliminado recursos preexistentes ni afirmo que toda la cuenta esté libre de cargos.

## Limitaciones de la verificación

Las pruebas HTTP y visuales confirman la aplicación servida; no abrí una sesión SSH ni ejecuté `docker ps` dentro de EC2. CloudFormation puede finalizar antes que UserData: esperé el arranque y volví a comprobar HTTP de forma acotada. No consulté una factura final ni modifiqué IAM. Los comandos auxiliares y los resultados con identificadores reales permanecen en `.local/`, excluidos de Git.
