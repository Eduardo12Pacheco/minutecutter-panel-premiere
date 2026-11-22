# Desarrollo de MinuteCutter

Pruebas, integración continua, estructura del repositorio y convenciones para contribuir.

## Pruebas

```bash
npm test
```

El comando ejecuta `node tests/harness.js`, un harness de comportamiento del modo sin ripple. Al terminar bien muestra `harness no-ripple: 25 passed, 0 failed`. Requiere Node.js 22 o superior (`engines` en `package.json`).

## Integración continua

`.github/workflows/ci.yml` ejecuta `npm test` con Node.js 22 en cada push a `main` y en cada pull request.

## Estructura del repositorio

```
minutecutter-panel-premiere/
├── .github/workflows/ci.yml     # CI: Node 22 + npm test
├── docs/
│   ├── assets/readme/           # heroes y diagramas (SVG)
│   ├── instalacion.md           # instalación para personas y agentes
│   ├── uso.md                   # uso, tiempos, mensajes y solución de problemas
│   └── desarrollo.md            # este documento
├── extension/
│   ├── CSXS/manifest.xml        # panel PPRO 22.x, CSXS 11, UI "Minute Cutter"
│   ├── client/                  # panel (index.html, index.css, index.js)
│   └── host/main.jsx            # lógica ExtendScript (ES3)
├── tests/harness.js             # harness del modo sin ripple
├── .gitignore
├── LICENSE                      # MIT
├── README.md                    # español
├── README.en.md                 # English
└── package.json                 # npm test
```

La ruta del host (`host/main.jsx`) se deriva en tiempo de ejecución desde la URL del panel. No hay rutas de usuario fijas en el código.

## Convenciones

- **Commits:** Conventional Commits (`feat`, `fix`, `docs`, `chore`, `ci`, `merge`), como en el historial actual.
- **Antes de cada commit,** revisa `git status` y `git diff`. No subas respaldos, archivos temporales ni secretos. El `.gitignore` ya excluye `node_modules/`, `*.bak`, `.env`, `backups/` y `logs/`.
- **Idioma:** los textos del panel y la documentación están en español; el código, los identificadores y los mensajes de commit están en inglés.
