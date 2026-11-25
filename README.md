# Laboratorio 15 — CloudFormation y Docker en Amazon EC2

Curso Desarrollo de Soluciones en la Nube — TECSUP.

## Estado inicial

He verificado Git, la autenticación de GitHub con permisos de escritura y el repositorio remoto vacío mediante `git ls-remote`. Reutilizo la carpeta seleccionada en Codex para evitar duplicados. La rama de trabajo es `main`.

Las evidencias originales se conservan localmente y se excluyen de Git. Las actividades AWS se documentarán únicamente después de verificarlas.

## Paso 1 — Plantilla validada

He creado `ec2-docker.yaml` y su copia `plantillas/01-nginx.yaml`. AWS CloudFormation validó la plantilla en `us-east-1`. Resolví la AMI oficial AL2023 x86_64 mediante SSM y comprobé la subred pública y ruta al Internet Gateway. Uso Docker con `dnf`, IMDSv2, disco gp3 cifrado de 8 GiB eliminable y créditos de CPU estándar. Mantengo SSH cerrado mientras no se necesite; HTTP 80 es público. No publico valores reales de red o cuenta.

La cuenta no devolvió beneficios EC2 en la consulta Free Tier. El usuario autorizó hasta US$1. La consulta AWS Pricing confirmó US$0.0116/h para t2.micro, más IPv4 y EBS.

## Paso 2 — Creación del stack

He cargado la plantilla y creado `lab-cloudformation-docker` mediante AWS Console en `us-east-1`, con t2.micro y una VPC y subred públicas existentes. Verifiqué `CREATE_IN_PROGRESS` mediante consola y CLI. No he creado una VPC ni un NAT Gateway. Las capturas de carga, parámetros y creación están guardadas localmente.

## Paso 3 — Recursos verificados

He comprobado `CREATE_COMPLETE`, los dos recursos del stack, la instancia `running`, los Outputs y la regla HTTP 80. IMDSv2 requiere token. No existe entrada SSH. Los valores reales de cuenta, IP e identificadores se conservan solo en evidencias locales excluidas de Git.

## Paso 4 — Nginx verificado

He abierto la IP vigente en Chrome y comprobado “Welcome to nginx!”. `curl` devolvió `HTTP/1.1 200 OK`, con servidor Nginx. La consulta inicial no conectó mientras arrancaba UserData; la siguiente fue exitosa sin cambiar la infraestructura. No he accedido por SSH a esta instancia.
