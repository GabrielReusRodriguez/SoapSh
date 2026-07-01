# SoapSh - Documentación Completa

> **Versión:** 0.1  
> **Autor:** Gabriel Reus Rodríguez  
> **Licencia:** GPLv3  
> **Fecha de creación:** 2024

---

## 📋 Índice

1. [Descripción General](#descripción-general)
2. [Estructura del Proyecto](#estructura-del-proyecto)
3. [Requisitos del Sistema](#requisitos-del-sistema)
4. [Instalación](#instalación)
5. [Configuración](#configuración)
   - [Configuración Global](#configuración-global)
   - [Configuración de Servicios Web](#configuración-de-servicios-web)
6. [Uso](#uso)
   - [Sintaxis de Comandos](#sintaxis-de-comandos)
   - [Parámetros](#parámetros)
   - [Ejemplos Prácticos](#ejemplos-prácticos)
7. [Estructura de Archivos SOAP](#estructura-de-archivos-soap)
8. [Plantillas y Ejemplos](#plantillas-y-ejemplos)
9. [Registros (Logs)](#registros-logs)
10. [Validación de Respuestas](#validación-de-respuestas)
11. [Casos de Uso](#casos-de-uso)
12. [Solución de Problemas](#solución-de-problemas)
13. [Contribución](#contribución)
14. [Licencia](#licencia)
15. [Recursos Adicionales](#recursos-adicionales)

---

## 📖 Descripción General

**SoapSh** es un script parametrizable en Bash diseñado para realizar llamadas a servicios web SOAP de manera sencilla y flexible. El proyecto está en desarrollo activo y se construye durante tiempo libre, por lo que se aceptan ideas, colaboraciones y mejoras por parte de la comunidad.

### 🎯 Objetivos del Proyecto
- Proporcionar una herramienta **ligera y sencilla** para interactuar con servicios SOAP
- Utilizar **herramientas estándar de Unix** (Bash, curl) sin dependencias complejas
- Ser **parametrizable y configurable** para diferentes servicios web
- Mantener un **registro detallado** de todas las operaciones realizadas
- Soportar **múltiples versiones de SOAP** (actualmente v1.1 y v1.2)

### 🔧 Características Principales
- ✅ Llamadas SOAP mediante curl con configuración personalizable
- ✅ Soporte para autenticación básica (Basic Auth)
- ✅ Generación automática de envelopes SOAP según versión
- ✅ Registro detallado de requests, responses, headers y traces
- ✅ Validación de respuestas mediante scripts personalizados
- ✅ Soporte para certificados SSL/TLS
- ✅ Modo verbose para depuración

---

## 🗂️ Estructura del Proyecto

```
SoapSh/
├── README.md                  # Documentación principal
├── LICENSE.md                # Licencia GPLv3
├── TODO.txt                  # Tareas pendientes y mejoras futuras
├── soapSh.sh                 # Script principal de ejecución
├── config/
│   └── config.cfg            # Configuración global de la aplicación
├── include/
│   └── functions.sh          # Funciones auxiliares (help, verbose, etc.)
├── utils/
│   └── readPEM.sh            # Utilidad para leer certificados PEM
├── SoapData/
│   ├── v1.1/
│   │   ├── soapEnvelope_begin.xml
│   │   ├── soapEnvelope_end.xml
│   │   ├── soapHeader_begin.xml
│   │   ├── soapHeader_end.xml
│   │   ├── soapBody_begin.xml
│   │   ├── soapBody_end.xml
│   │   └── schema.xsd
│   └── v1.2/
│       ├── soapEnvelope_begin.xml
│       ├── soapEnvelope_end.xml
│       ├── soapHeader_begin.xml
│       ├── soapHeader_end.xml
│       ├── soapBody_begin.xml
│       ├── soapBody_end.xml
│       └── schema.xsd
├── Logs/                     # Carpeta de registros (se crea dinámicamente)
└── webServices/
    ├── template/             # Plantilla base para nuevos servicios
    │   ├── ws.cfg            # Configuración específica del WS
    │   ├── template_curl.cfg # Configuración de curl
    │   ├── data/
    │   │   ├── soapHeaderPayload.xml
    │   │   └── soapBodyPayload.xml
    │   └── scripts/
    │       ├── valid.sh      # Script de validación
    │       └── README
    ├── calculator/           # Ejemplo: Servicio calculadora
    ├── sii/                  # Ejemplo: Servicio SII
    └── TeleseguimentApp/     # Ejemplo: Aplicación de teleseguimiento
```

---

## 🖥️ Requisitos del Sistema

### Sistema Operativo
- **Soporte:** Sistemas Unix/Linux (probado en Kubuntu 20)
- **No compatible:** Windows (requiere Bash y herramientas Unix)

### Dependencias Obligatorias
| Herramienta | Versión Mínima | Descripción |
|-------------|----------------|-------------|
| **Bash** | 4.0+ | Intérprete de comandos |
| **curl** | 7.0+ | Cliente HTTP para realizar las peticiones |
| **openssl** | 1.0+ | Para manejo de certificados (opcional) |

### Verificación de Dependencias
```bash
# Verificar versión de Bash
bash --version

# Verificar versión de curl
curl --version

# Verificar versión de openssl
openssl version
```

---

## 📥 Instalación

### 1. Clonar el Repositorio
```bash
git clone https://github.com/GabrielReusRodriguez/SoapSh.git
cd SoapSh
```

### 2. Asignar Permisos de Ejecución
```bash
chmod +x soapSh.sh
chmod +x utils/readPEM.sh
chmod +x webServices/template/scripts/valid.sh
```

### 3. Configuración Inicial
El proyecto ya incluye una configuración por defecto en `config/config.cfg`. Puedes personalizarla según tus necesidades.

---

## ⚙️ Configuración

### Configuración Global (`config/config.cfg`)

```bash
#!/bin/bash

# Constantes de estado
CFG_CONST_OK="OK"
CFG_CONST_FAIL="FAIL"

# Configuración SOAP
CFG_SOAP_ENVELOPE_BEGIN="soapEnvelope_begin.xml"
CFG_SOAP_HEADER_BEGIN="soapHeader_begin.xml"
CFG_SOAP_HEADER_END="soapHeader_end.xml"
CFG_SOAP_BODY_BEGIN="soapBody_begin.xml"
CFG_SOAP_BODY_END="soapBody_end.xml"
CFG_SOAP_ENVELOPE_END="soapEnvelope_end.xml"

# Carpeta de Logs
CFG_LOG_FOLDER="${FOLDER}/Logs"

# Metadatos
CFG_VERSION="0.1"
CFG_AUTHOR="gabriel.reusrodriguez@csi.cat"
```

**Variables personalizables:**
- `CFG_LOG_FOLDER`: Ruta donde se guardarán los logs
- `CFG_VERSION`: Versión de la aplicación
- `CFG_AUTHOR`: Autor del script

### Configuración de Servicios Web

Cada servicio web tiene su propia carpeta dentro de `webServices/` con los siguientes archivos:

#### 1. `ws.cfg` - Configuración del Servicio
```bash
#!/bin/bash

# Nombre del servicio
WS_NAME="Template"

# Ruta al archivo de configuración de curl
CURL_FILE="${WS_DIR}/${WS_NAME}_curl.cfg"

# Versión SOAP a utilizar
SOAP_VERSION="v1.1"

# Rutas a los payloads (header y body)
SOAP_HEADER_PAYLOAD="${WS_DIR}/data/soapHeaderPayload.xml"
SOAP_BODY_PAYLOAD="${WS_DIR}/data/soapBodyPayload.xml"

# Credenciales (opcional)
# SOAP_USER="usuario:contraseña"

# Validador de respuesta (opcional)
# VALIDATOR="${WS_DIR}/scripts/valid.sh"
```

#### 2. `<nombre>_curl.cfg` - Configuración de curl
```bash
# Endpoint del servicio
url = "http://ejemplo.com/servicio"

# Headers HTTP
header = Content-Type:text/xml;charset=UTF-8
header = SOAPAction:""
header = Connection:Keep-Alive

# Timeouts
connect-timeout = 10
max-time = 500

# Opciones adicionales
verbose
include
```

**Opciones de curl soportadas:**
- `url`: Endpoint del servicio SOAP
- `header`: Headers HTTP personalizados
- `connect-timeout`: Tiempo máximo para establecer conexión (segundos)
- `max-time`: Tiempo máximo para la operación completa (segundos)
- `verbose`: Activa modo detallado
- `include`: Incluye headers en el output
- `cacert`: Ruta al certificado CA
- `capath`: Ruta a la carpeta de certificados

---

## 🚀 Uso

### Sintaxis de Comandos

```bash
./soapSh.sh [OPCIONES]
```

### Parámetros

| Parámetro | Abreviatura | Requerido | Descripción |
|-----------|-------------|-----------|-------------|
| `--name <nombre>` | `-n <nombre>` | ✅ | Nombre del servicio web a invocar |
| `--curl-cfg <ruta>` | - | ❌ | Ruta al archivo de configuración de curl |
| `--ws-dir <ruta>` | - | ✅ | Carpeta que contiene la configuración del WS (ws.cfg y <X>.cfg) |
| `--soap-version <versión>` | - | ❌ | Versión SOAP a utilizar (v1.1 o v1.2). Por defecto: v1.1 |
| `--user <credenciales>` | `-u <credenciales>` | ❌ | Credenciales para autenticación básica (formato: usuario:contraseña) |
| `--verbose` | `-v` | ❌ | Activa el modo verbose para depuración |
| `--help` | `-h` | ❌ | Muestra la ayuda |

### Ejemplos Prácticos

#### Ejemplo 1: Llamada básica a un servicio
```bash
./soapSh.sh --name Template --ws-dir ./webServices/template
```

#### Ejemplo 2: Llamada con autenticación básica
```bash
./soapSh.sh --name Template --ws-dir ./webServices/template \
  --user gabriel:micontraseña123
```

#### Ejemplo 3: Llamada con versión SOAP específica
```bash
./soapSh.sh --name Template --ws-dir ./webServices/template \
  --soap-version v1.2
```

#### Ejemplo 4: Llamada con configuración de curl personalizada
```bash
./soapSh.sh --name Template --ws-dir ./webServices/template \
  --curl-cfg ./webServices/template/custom_curl.cfg
```

#### Ejemplo 5: Llamada con modo verbose
```bash
./soapSh.sh --name Template --ws-dir ./webServices/template --verbose
```

#### Ejemplo 6: Llamada completa con todas las opciones
```bash
./soapSh.sh --name Calculator --ws-dir ./webServices/calculator \
  --soap-version v1.2 \
  --user admin:password123 \
  --curl-cfg ./webServices/calculator/calculator_curl.cfg \
  --verbose
```

---

## 📄 Estructura de Archivos SOAP

### Versiones Soportadas
- **SOAP 1.1**: Estándar más común
- **SOAP 1.2**: Versión mejorada con namespace XML

### Archivos Base por Versión

#### SOAP v1.1

**soapEnvelope_begin.xml:**
```xml
<soapenv:Envelope
```

**soapEnvelope_end.xml:**
```xml
</soapenv:Envelope>
```

**soapHeader_begin.xml:**
```xml
<soapenv:Header>
```

**soapHeader_end.xml:**
```xml
</soapenv:Header>
```

**soapBody_begin.xml:**
```xml
<soapenv:Body>
```

**soapBody_end.xml:**
```xml
</soapenv:Body>
```

#### SOAP v1.2

**soapEnvelope_begin.xml:**
```xml
<soap:Envelope xmlns:soap="http://www.w3.org/2003/05/soap-envelope"
```

**soapEnvelope_end.xml:**
```xml
</soap:Envelope>
```

Los archivos de payload (`soapHeaderPayload.xml` y `soapBodyPayload.xml`) deben ser proporcionados por el usuario en la carpeta `data/` de cada servicio.

---

## 📁 Plantillas y Ejemplos

### Plantilla Base (`webServices/template/`)

La plantilla incluye:
- `ws.cfg`: Configuración básica del servicio
- `template_curl.cfg`: Configuración de curl de ejemplo
- `data/`: Carpeta con payloads de ejemplo
- `scripts/`: Scripts de validación

### Ejemplo: Servicio Calculator

El servicio `calculator` (en `webServices/calculator/`) demuestra cómo configurar un servicio real:

```bash
# Ejemplo de llamada al servicio calculator
./soapSh.sh --name calculator --ws-dir ./webServices/calculator
```

### Crear un Nuevo Servicio

1. **Crear la estructura de directorios:**
```bash
mkdir -p webServices/miservicio/data
mkdir -p webServices/miservicio/scripts
```

2. **Crear `ws.cfg`:**
```bash
#!/bin/bash
WS_NAME="miservicio"
CURL_FILE="${WS_DIR}/${WS_NAME}_curl.cfg"
SOAP_VERSION="v1.1"
SOAP_HEADER_PAYLOAD="${WS_DIR}/data/soapHeaderPayload.xml"
SOAP_BODY_PAYLOAD="${WS_DIR}/data/soapBodyPayload.xml"
```

3. **Crear `<nombre>_curl.cfg`:**
```bash
url = "https://ejemplo.com/soap"
header = Content-Type:text/xml;charset=UTF-8
header = SOAPAction:"urn:ejemplo#metodo"
connect-timeout = 30
max-time = 60
```

4. **Crear payloads XML:**
- `data/soapHeaderPayload.xml`: Contenido del header SOAP
- `data/soapBodyPayload.xml`: Contenido del body SOAP

5. **Probar el servicio:**
```bash
./soapSh.sh --name miservicio --ws-dir ./webServices/miservicio
```

---

## 📝 Registros (Logs)

### Estructura de Logs

Para cada ejecución, se crea una carpeta en `Logs/` con el siguiente formato:
```
Logs/
└── <nombreWS>_<timestamp>/
    ├── <nombreWS>_<timestamp>_Req_Payload.log    # Request XML enviado
    ├── <nombreWS>_<timestamp>_Req_Headers.hdr    # Headers del request
    ├── <nombreWS>_<timestamp>_Resp_Payload.log   # Response XML recibido
    ├── <nombreWS>_<timestamp>_Resp_Headers.hdr   # Headers del response
    ├── <nombreWS>_<timestamp>.err                # Errores de curl
    ├── <nombreWS>_<timestamp>.trc                # Trace completo de la conexión
    └── <nombreWS>_<timestamp>.val                # Resultado de la validación
```

### Formato del Timestamp
El timestamp sigue el formato: `YYYY-MM-DD_HH-MM-SS`

Ejemplo: `Template_2024-07-01_14-30-45`

### Contenido de los Archivos de Log

#### Req_Payload.log
Contiene el XML completo del request SOAP generado:
```xml
<soapenv:Envelope>
  <soapenv:Header>
    [Contenido del header]
  </soapenv:Header>
  <soapenv:Body>
    [Contenido del body]
  </soapenv:Body>
</soapenv:Envelope>
```

#### Resp_Payload.log
Contiene el XML completo del response SOAP recibido.

#### .err
Contiene los errores generados por curl (si los hay).

#### .trc
Contiene el trace completo de la conexión HTTP, incluyendo:
- Request headers
- Request body
- Response headers
- Response body
- Información de SSL/TLS (si aplica)

---

## ✅ Validación de Respuestas

### Configuración de Validación

En el archivo `ws.cfg` de cada servicio, puedes especificar un script de validación:
```bash
VALIDATOR="${WS_DIR}/scripts/valid.sh"
```

### Script de Validación de Ejemplo

El script `valid.sh` en `webServices/template/scripts/` es un ejemplo básico:

```bash
#!/bin/bash
# Script de validación personalizado
# Este script recibe el archivo de respuesta y debe devolver:
# - 0 si la validación es exitosa
# - 1 si hay errores

# Ejemplo: Validar que el response contiene "OK"
if grep -q "OK" "${LOG_FILE_RESP_PAYLOAD}"; then
    echo "Validación exitosa: Response contiene 'OK'"
    exit 0
else
    echo "Validación fallida: Response no contiene 'OK'"
    exit 1
fi
```

### Crear un Validador Personalizado

1. **Crear el script en la carpeta `scripts/`:**
```bash
nano webServices/miservicio/scripts/valid.sh
```

2. **Hacerlo ejecutable:**
```bash
chmod +x webServices/miservicio/scripts/valid.sh
```

3. **Referenciarlo en `ws.cfg`:**
```bash
VALIDATOR="${WS_DIR}/scripts/valid.sh"
```

4. **El script puede acceder a las siguientes variables:**
   - `LOG_FILE_RESP_PAYLOAD`: Ruta al archivo con el response
   - `LOG_FILE_RESP_HEADERS`: Ruta al archivo con los headers del response
   - `LOG_FILE_VALIDATION`: Ruta donde guardar el resultado de la validación

---

## 🎯 Casos de Uso

### Caso 1: Consumo de un Servicio SOAP Público

**Objetivo:** Invocar un servicio SOAP público como el de calculadora.

**Pasos:**
1. Configurar el endpoint en `webServices/calculator/calculator_curl.cfg`
2. Definir los payloads XML en `data/`
3. Ejecutar:
```bash
./soapSh.sh --name calculator --ws-dir ./webServices/calculator
```

### Caso 2: Servicio con Autenticación Básica

**Objetivo:** Invocar un servicio SOAP que requiere autenticación.

**Pasos:**
1. Configurar el servicio en `ws.cfg`
2. Pasar credenciales por parámetro:
```bash
./soapSh.sh --name miservicio --ws-dir ./webServices/miservicio \
  --user miusuario:micontraseña
```

### Caso 3: Servicio con Certificados SSL

**Objetivo:** Invocar un servicio SOAP sobre HTTPS con certificados.

**Pasos:**
1. Configurar en `curl.cfg`:
```bash
url = "https://seguro.ejemplo.com/soap"
cacert = /ruta/a/certificado.pem
capath = /ruta/a/carpeta/certs
```
2. Ejecutar normalmente:
```bash
./soapSh.sh --name seguro --ws-dir ./webServices/seguro
```

### Caso 4: Depuración de un Servicio

**Objetivo:** Depurar una llamada SOAP para identificar problemas.

**Pasos:**
1. Activar modo verbose:
```bash
./soapSh.sh --name problema --ws-dir ./webServices/problema --verbose
```
2. Revisar los archivos en `Logs/`:
   - `.trc` para el trace completo
   - `.err` para errores
   - `.hdr` para headers

### Caso 5: Validación Automática de Respuestas

**Objetivo:** Validar automáticamente que la respuesta contiene ciertos datos.

**Pasos:**
1. Crear un script de validación en `scripts/valid.sh`
2. Configurar `VALIDATOR` en `ws.cfg`
3. Ejecutar el servicio:
```bash
./soapSh.sh --name validado --ws-dir ./webServices/validado
```
4. Revisar el archivo `.val` en la carpeta de logs

---

## 🐛 Solución de Problemas

### Problemas Comunes

#### 1. Error: "No se pudo cargar el fichero de configuración"
**Causa:** El archivo `config/config.cfg` no existe o no es accesible.
**Solución:**
```bash
# Verificar que el archivo existe
ls -la config/config.cfg

# Verificar permisos
chmod 644 config/config.cfg
```

#### 2. Error: "No se ha encontrado configuración de CURL"
**Causa:** No se proporcionó el parámetro `--curl-cfg` o el archivo no existe.
**Solución:**
```bash
# Verificar que el archivo de configuración de curl existe
ls -la webServices/<servicio>/*curl.cfg

# Proporcionar el parámetro correctamente
./soapSh.sh --name <servicio> --ws-dir ./webServices/<servicio> --curl-cfg ./webServices/<servicio>/<servicio>_curl.cfg
```

#### 3. Error: "the curl command failed with: <código>"
**Causas comunes:**
- Endpoint incorrecto
- Problemas de conexión
- Timeout excedido
- Error SSL/TLS

**Solución:**
```bash
# Activar modo verbose para más detalles
./soapSh.sh --name <servicio> --ws-dir ./webServices/<servicio> --verbose

# Revisar el archivo .err en la carpeta de logs
cat Logs/<servicio>_<timestamp>/*err

# Revisar el trace completo
cat Logs/<servicio>_<timestamp>/*trc
```

#### 4. Error SSL: "SSL certificate problem"
**Causa:** El certificado SSL del servidor no es de confianza.
**Soluciones:**

**Opción A:** Desactivar verificación SSL (no recomendado para producción):
```bash
# En el archivo curl.cfg, agregar:
k = true
```

**Opción B:** Proporcionar certificado CA:
```bash
# En el archivo curl.cfg, agregar:
cacert = /ruta/a/certificado.pem
```

**Opción C:** Proporcionar carpeta de certificados:
```bash
# En el archivo curl.cfg, agregar:
capath = /ruta/a/carpeta/certs
```

#### 5. Error: "No such file or directory" al crear logs
**Causa:** La carpeta `Logs/` no existe o no tiene permisos de escritura.
**Solución:**
```bash
# Crear la carpeta
mkdir -p Logs

# Asignar permisos
chmod 755 Logs
```

### Depuración Avanzada

#### Verificar la estructura del request SOAP
```bash
# Ejecutar con verbose
./soapSh.sh --name <servicio> --ws-dir ./webServices/<servicio> --verbose

# Ver el request generado
cat Logs/<servicio>_<timestamp>/*Req_Payload.log
```

#### Probar curl manualmente
```bash
# Extraer el request de los logs
REQUEST=$(cat Logs/<servicio>_<timestamp>/*Req_Payload.log)

# Ejecutar curl manualmente
curl --config webServices/<servicio>/<servicio>_curl.cfg \
  --data-binary @Logs/<servicio>_<timestamp>/*Req_Payload.log \
  --verbose
```

#### Validar XML con xmllint
```bash
# Instalar xmllint (si no está instalado)
sudo apt-get install libxml2-utils

# Validar el request XML
xmllint --format Logs/<servicio>_<timestamp>/*Req_Payload.log --noout
```

---

## 🤝 Contribución

### Cómo Contribuir

1. **Fork el repositorio** en GitHub
2. **Crea una rama** para tu funcionalidad:
   ```bash
   git checkout -b feature/nueva-funcionalidad
   ```
3. **Realiza tus cambios** y haz commit:
   ```bash
   git commit -m "Añade nueva funcionalidad"
   ```
4. **Envía un Pull Request** a la rama `master`

### Estándares de Código

- **Bash:** Seguir las mejores prácticas de scripting en Bash
- **Nombres de archivos:** Usar minúsculas y guiones (`mi-archivo.sh`)
- **Comentarios:** Documentar funciones y lógica compleja
- **Logs:** Mantener el formato de logs consistente

### Reportar Problemas

1. **Verificar que el problema no esté reportado** en los Issues
2. **Proporcionar información detallada:**
   - Versión de Bash y curl
   - Pasos para reproducir
   - Mensaje de error exacto
   - Archivos de configuración relevantes
   - Logs de la ejecución

---

## 📜 Licencia

Este proyecto está bajo la **Licencia Pública General GNU versión 3 (GPLv3)**.

### Resumen de la Licencia
- ✅ **Libertad 0:** Usar el programa para cualquier propósito
- ✅ **Libertad 1:** Estudiar cómo funciona el programa y modificarlo
- ✅ **Libertad 2:** Distribuir copias del programa
- ✅ **Libertad 3:** Distribuir versiones modificadas del programa

### Obligaciones
- Incluir el archivo `LICENSE.md` en todas las distribuciones
- Mantener los avisos de copyright
- Publicar el código fuente de las versiones modificadas

### Texto Completo
Consulta el archivo [LICENSE.md](LICENSE.md) para el texto completo de la licencia.

---

## 🔗 Recursos Adicionales

### Documentación Externa
- [Documentación de curl](https://curl.se/docs/)
- [Especificación SOAP 1.1](https://www.w3.org/TR/soap11/)
- [Especificación SOAP 1.2](https://www.w3.org/TR/soap12/)
- [Guía de Bash](https://www.gnu.org/software/bash/manual/)

### Herramientas Útiles
- **SoapUI:** Herramienta gráfica para probar servicios SOAP
- **Postman:** Alternativa para probar APIs (con soporte SOAP)
- **xmllint:** Validador de XML
- **openssl:** Herramienta para manejo de certificados

### Wiki del Proyecto
Puedes encontrar información adicional, ejemplos y guías detalladas en la [Wiki del proyecto](https://github.com/GabrielReusRodriguez/SoapSh/wiki).

---

## 📞 Soporte

### Contacto
- **Autor:** Gabriel Reus Rodríguez
- **Email:** gabriel.reusrodriguez@csi.cat
- **GitHub:** [@GabrielReusRodriguez](https://github.com/GabrielReusRodriguez)

### Comunidad
- **Issues:** [GitHub Issues](https://github.com/GabrielReusRodriguez/SoapSh/issues)
- **Discusiones:** [GitHub Discussions](https://github.com/GabrielReusRodriguez/SoapSh/discussions)

---

## 🏁 Conclusión

**SoapSh** es una herramienta poderosa y flexible para interactuar con servicios SOAP desde la línea de comandos. Su diseño modular permite adaptarse a diferentes escenarios, desde pruebas rápidas hasta integraciones en scripts de producción.

**¡Contribuciones son bienvenidas!** Si tienes ideas para mejorar el proyecto, no dudes en abrir un Issue o enviar un Pull Request.

---

*Documentación generada el 1 de julio de 2024. Última actualización: Versión 0.1*
