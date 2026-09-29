# Mini landing + Docker + pipeline

Carpeta lista para subir a un repo de GitHub.

```
mini-docker-page/
├── index.html                 # Landing
├── Dockerfile                 # Imagen nginx
├── docker-compose.yml         # Levantar en local con un comando
├── .dockerignore
├── .gitignore
└── .github/workflows/
    └── pipeline.yml           # CI + despliegue Pages (solo push a main)
```

## Subir a un repo nuevo en GitHub

1. Crea un repo vacío en GitHub (sin README).
2. En PowerShell, desde esta carpeta:

```powershell
cd C:\Users\SHIRLEYMORALES\mini-docker-page
git add .
git commit -m "Mini landing con Docker y pipeline"
git remote add origin https://github.com/TU_USUARIO/TU_REPO.git
git push -u origin main
```

3. En el repo: **Settings → Pages → Build and deployment → Source: GitHub Actions**.

Tras el primer push a `main`, el workflow **Pipeline** corre solo.

## Qué hace el pipeline (y qué no)

| Paso | Dónde ocurre | ¿Puedes abrirlo en el navegador? |
|------|----------------|-----------------------------------|
| `docker build` + contenedor + `curl` | Máquina efímera de GitHub Actions | **No.** El contenedor vive unos segundos solo para la prueba y se apaga. |
| Job `deploy-pages` (push a `main`) | GitHub Pages | **Sí.** Misma `index.html`, servida por Pages (no por Docker en la nube). |

En **Actions** verás el job en verde; en **Deployments** (o en el job `deploy-pages`) aparece la URL, algo como:

`https://TU_USUARIO.github.io/TU_REPO/`

Esa es la forma de **ver el landing en internet** después de que el pipeline termine bien.

## Ver el landing con Docker (como en el pipeline)

En tu PC, después de clonar o desde esta carpeta:

```powershell
docker compose up --build
```

Abre **http://localhost:8080**.

Equivalente sin Compose:

```powershell
docker build -t mini-docker-page .
docker run --rm -p 8080:80 mini-docker-page
```

## Resumen

- **Repo:** sube todo el contenido de `mini-docker-page`.
- **Tras el pipeline en GitHub:** activa Pages + mira la URL de GitHub Pages.
- **Tras el pipeline en local:** `docker compose up --build` → `http://localhost:8080`.
