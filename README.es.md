# HCL DX Compose Labs · Asistente de configuración

[English](README.md) · **Español**

Código fuente del **asistente de configuración opcional** del repositorio
[hcl-dx-compose-labs](https://github.com/europesip/hcl-dx-compose-labs) (HCL DX Compose CF238
instalado desde cero en Kubernetes, a modo de PoC).

- **Úsalo en línea (GitHub Pages):** empieza por la [página de presentación](https://europesip.github.io/hcl-dx-compose-labs/about.html?lang=es)
  o ve directamente al [asistente](https://europesip.github.io/hcl-dx-compose-labs/?lang=es).
- **O ejecútalo en local:** descarga [`index.html`](index.html) (el asistente) y [`about.html`](about.html)
  (las explicaciones) y ábrelos en tu navegador. Funcionan sin conexión.

| Fichero | Qué es |
|---|---|
| [`about.html`](about.html) | Página de presentación: qué hace el asistente, las dos opciones de salida, la privacidad y cómo comprobarla. Al final: *continuar al asistente* o *seguir los labs a mano* |
| [`index.html`](index.html) | El asistente en sí: el formulario, la generación de `env.sh` / `RUNBOOK.md` y el ZIP. Toda la lógica está aquí |

> ⚠️ **Experimental · en desarrollo (WIP).** El asistente evoluciona constantemente y debe tratarse
> como algo experimental. Busca facilitar y aclarar la instalación, pero **hoy la vía recomendada
> es la que no usa el asistente**: `cp env.example.sh env.sh`, editarlo y seguir las guías de cada
> lab.

## Qué hace

Pregunta tu plataforma (Kubernetes, OpenShift o la PoC en WSL2), el nombre de host de DX, las
StorageClasses, los namespaces y nombres de releases y los labs que quieres hacer (todos los campos
rellenos con los valores del entorno de referencia de los labs; solo las credenciales de Harbor y
OpenRouter están vacías), y te da tres opciones:

| Opción | Obtienes | Después |
|---|---|---|
| **A · Solo `env.sh`** | El fichero de configuración que necesitan los labs | Sigues los labs a mano con sus guías |
| **B · `env.sh` + `RUNBOOK.md`** (ZIP) | Además un `RUNBOOK.md` personalizado y **de solo lectura**: las instrucciones para montar, uno tras otro, los labs que has elegido, con los scripts que hay que ejecutar y los comandos `kubectl` / `helm` equivalentes con tus valores | Sigues el runbook |
| **C · Instalación directa** (ZIP con `runbook/`) | Un `RUNBOOK.md` optimizado con **comandos directos** (tus valores escritos en cada comando, las comprobaciones marcadas como opcionales), un `install.sh` que ejecuta esos mismos comandos uno tras otro mostrando el lab y la fase, `secrets.env` (los únicos valores que no van escritos en los comandos) y los values generados | `cd runbook`, rellena `secrets.env`, sigue `RUNBOOK.md` o ejecuta `./install.sh` (`--list`, `--verify`, `--from <paso>`) |

A y B van en la raíz de tu copia del repositorio de los labs y usan los scripts de los labs. C
también se descomprime en la raíz de la copia, pero crea su propia carpeta `runbook/`, no
sobrescribe nada y solo lee los ficheros de los labs. El asistente en sí nunca habla con tu clúster.

## Por qué puedes confiarle tus credenciales

El asistente está pensado para que cualquiera pueda comprobar que **las credenciales siguen siendo
secretas y no se guardan en ningún sitio**:

- **Un único fichero.** Toda la lógica del asistente está en [`index.html`](index.html) (`about.html` solo
  tiene texto y el cambio de idioma): HTML, CSS y JavaScript sencillos,
  **sin React, sin frameworks, sin compilación y sin dependencias**. Lo que lees es exactamente lo
  que se ejecuta, y es fácil de revisar. Por eso también funciona tal cual en GitHub Pages.
- **Sin red, garantizado por el navegador.** Las dos páginas declaran una Content-Security-Policy:
  `default-src 'none'; connect-src 'none'; script-src 'unsafe-inline'; ...` El navegador bloquea
  cualquier petición (`fetch`, XHR, WebSocket, scripts, imágenes o fuentes externas): aunque el
  código intentara enviar algo, no podría.
- **Todo ocurre en tu navegador.** `env.sh` y `RUNBOOK.md` se generan en memoria y se descargan con
  un enlace local `blob:`; el ZIP lo monta una pequeña función del mismo fichero.
- **Los secretos son opcionales.** El token de Harbor, la clave de OpenRouter y las contraseñas se
  pueden dejar vacíos y rellenarlos después en `env.sh` (que está en el `.gitignore` de los labs:
  nunca se sube a git).
- **Solo se guarda una cosa**: el idioma elegido, en `localStorage`.

Cómo comprobarlo tú mismo:

1. Lee `index.html` (el JavaScript está al final, unas 500 líneas comentadas).
2. Búscalo: no hay `fetch`, `XMLHttpRequest`, `WebSocket`, `sendBeacon` ni `src=`/`href=` externos a scripts o estilos.
   ```bash
   grep -nE 'fetch\(|XMLHttpRequest|WebSocket|sendBeacon|<script src|<link ' index.html
   ```
3. Abre las herramientas de desarrollo del navegador, pestaña **Red** (*Network*), rellena el formulario y descarga: no hay peticiones.
4. O ejecútalo en local con la red desconectada.

## Sobre este asistente

Es una **opción simplificada y funcional, específica de estos labs**. EUROPESIP tiene otro
asistente desarrollado internamente, con funciones avanzadas y más opciones de personalización,
disponible bajo demanda: https://www.europesip.com

La copia publicada en GitHub Pages está en el repositorio de los labs
([docs/](https://github.com/europesip/hcl-dx-compose-labs/tree/main/docs)) y
son los mismos ficheros que estos.
