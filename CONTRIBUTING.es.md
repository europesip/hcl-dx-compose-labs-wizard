# Cómo contribuir

[English](CONTRIBUTING.md) · **Español**

El asistente es código abierto (Apache License 2.0, ver [LICENSE](LICENSE) y [NOTICE](NOTICE)) y
cualquiera puede ayudar a mejorarlo: abre una [issue](https://github.com/europesip/hcl-dx-compose-labs-wizard/issues)
o una pull request.

- Mantén **una página estática por fichero**, con HTML, CSS y JavaScript sencillos: sin frameworks, sin
  compilación, sin dependencias y sin acceso a red (la Content-Security-Policy debe seguir con
  `connect-src 'none'`). Es lo que permite a cualquiera comprobar que las credenciales nunca salen del navegador.
- Textos en **inglés y español**.
- Conserva el fichero [NOTICE](NOTICE) y la atribución a EUROPESIP como autor original.

**Dónde cambiarlo.** La copia publicada está en el repositorio de los labs
([`docs/`](https://github.com/europesip/hcl-dx-compose-labs/tree/main/docs)), que sirve GitHub Pages, y las
dos copias tienen que ser idénticas. El bloque `LAB TEMPLATES` de `index.html` (los ficheros de los labs
que usa la opción C) se genera con `tools/sync-wizard-templates.py` en el repositorio de los labs: no lo
edites a mano. El procedimiento completo está en el
[CONTRIBUTING.es.md de los labs](https://github.com/europesip/hcl-dx-compose-labs/blob/main/CONTRIBUTING.es.md#mantener-el-asistente-de-configuración).
