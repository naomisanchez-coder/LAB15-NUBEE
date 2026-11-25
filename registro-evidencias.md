# Registro de evidencias

Las capturas originales permanecen locales y no se publican. Solo se registran archivos existentes y revisados.

| Número | Paso de la guía | Archivo | Ruta relativa | Descripción académica | Verificación |
|---|---|---|---|---|---|
| 1 | 00-inicializacion | 01_cloudformation_sin_stacks.png | evidencias/00-inicializacion/01_cloudformation_sin_stacks.png | CloudFormation sin stacks previos | Capturada y revisada |
| 2 | 00-inicializacion | 02_proyecto_vscode.png | evidencias/00-inicializacion/02_proyecto_vscode.png | Proyecto independiente abierto en VS Code | Capturada y revisada |
| 3 | 01-plantilla | 03_plantilla_vscode.png | evidencias/01-plantilla/03_plantilla_vscode.png | Plantilla AL2023 y reglas HTTP | Capturada y revisada |
| 4 | 01-plantilla | 04_validacion_aws.png | evidencias/01-plantilla/04_validacion_aws.png | Resultado real de validate-template | Capturada y revisada |
| 5 | Paso 2 | 05_carga_plantilla.png | evidencias/02-creacion-stack/05_carga_plantilla.png | Carga real de ec2-docker.yaml | Capturada y revisada |
| 6 | Paso 2 | 06_nombre_parametros.png | evidencias/02-creacion-stack/06_nombre_parametros.png | Nombre exacto y parametros de red compatibles | Capturada y revisada |
| 7 | Paso 2 | 07_creacion_iniciada.png | evidencias/02-creacion-stack/07_creacion_iniciada.png | Estado CREATE_IN_PROGRESS iniciado desde consola | Capturada y revisada |
| 8 | Paso 3 | 08_stack_create_complete.png | evidencias/03-verificacion-recursos/08_stack_create_complete.png | Estado CREATE_COMPLETE real | Capturada y revisada |
| 9 | Paso 3 | 09_recursos_cloudformation.png | evidencias/03-verificacion-recursos/09_recursos_cloudformation.png | Instancia y Security Group creados | Capturada y revisada |
| 10 | Paso 3 | 10_outputs.png | evidencias/03-verificacion-recursos/10_outputs.png | Outputs InstanceId y PublicIP vigentes | Capturada y revisada |
| 11 | Paso 3 | 11_ec2_running.png | evidencias/03-verificacion-recursos/11_ec2_running.png | EC2 t2.micro en ejecucion con IMDSv2 | Capturada y revisada |
| 12 | Paso 3 | 12_security_group.png | evidencias/03-verificacion-recursos/12_security_group.png | Unica regla de entrada TCP 80; SSH cerrado | Capturada y revisada |
| 13 | Paso 4 | 13_nginx_funcionando.png | evidencias/04-despliegue-nginx/13_nginx_funcionando.png | Welcome to nginx! en navegador | Capturada y revisada |
| 14 | Paso 4 | 14_http_200_nginx.png | evidencias/04-despliegue-nginx/14_http_200_nginx.png | Cabeceras reales obtenidas mediante curl: HTTP 200 | Capturada y revisada |
| 15 | 00-inicializacion | 03_github_main_publicado.png | evidencias/00-inicializacion/03_github_main_publicado.png | Repositorio GitHub en main con cinco commits publicados | Capturada y revisada |
| 16 | 05-actualizacion-stack | 15_hello_yaml.png | evidencias/05-actualizacion-stack/15_hello_yaml.png | UserData hello e identificador logico nuevo | Capturada y revisada |
| 17 | 05-actualizacion-stack | 16_cambios_reemplazo.png | evidencias/05-actualizacion-stack/16_cambios_reemplazo.png | Vista previa Add WebServerHello y Remove WebServer | Capturada y revisada |
| 18 | 05-actualizacion-stack | 17_update_complete.png | evidencias/05-actualizacion-stack/17_update_complete.png | UPDATE_COMPLETE real de hello | Capturada y revisada |
| 19 | 05-actualizacion-stack | 18_nginx_hello_funcionando.png | evidencias/05-actualizacion-stack/18_nginx_hello_funcionando.png | Aplicacion Hello World con datos del contenedor | Capturada y revisada |
| 20 | Desafio 1 | 19_tags_yaml.png | evidencias/06-desafio-tags/19_tags_yaml.png | Etiqueta Name exacta en YAML | Capturada y revisada |
| 21 | Desafio 1 | 20_tags_update_complete.png | evidencias/06-desafio-tags/20_tags_update_complete.png | UPDATE_COMPLETE de etiquetas | Capturada y revisada |
| 22 | Desafio 1 | 21_tags_ec2.png | evidencias/06-desafio-tags/21_tags_ec2.png | Name = Laboratorio-Docker visible en EC2 | Capturada y revisada |
