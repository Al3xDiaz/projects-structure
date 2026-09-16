# projects-structure

Este repositorio documenta cómo está organizado `~/projects`. **Solo versiona este `README.md`
y el `.gitignore`** — cada carpeta de abajo es un repositorio independiente, clonado por
separado, y queda ignorada aquí (ver [`.gitignore`](.gitignore)).

## Layout

```
~/projects/
├── frontend/
│   ├── portfolio/     # web personal
│   └── dashboard/      (org chaoticteam)
├── backend/
│   └── backend/         (org chaoticteams/chaoticteam)
├── mobile/
│   └── fit-swap/
└── utils/
    └── gitversion-template/
```

Convención: se agrupa por capa (`frontend/`, `backend/`, `mobile/`, `utils/`), no por
plataforma ni por dueño. Repos frontend van en `frontend/`, el backend en `backend/`,
independientemente de si viven en la cuenta personal o en una organización.

## Repos espejados en GitLab + GitHub

Tres de estos repos existen espejados en ambas plataformas y usan **push dual**: un solo
`git push origin` publica a GitLab y GitHub a la vez.

| Carpeta local | GitLab | GitHub |
|---|---|---|
| `frontend/portfolio` | `Al3xDiaz/portfolio` | `Al3xDiaz/portfolio` |
| `frontend/dashboard` | `chaoticteams/dashboard` | `chaoticteam/dashboard` |
| `backend/backend` | `chaoticteams/backend` | `chaoticteam/backend` |

⚠️ **Ojo con el nombre de la organización**: en GitLab es `chaoticteams` (con "s"), en GitHub
es `chaoticteam` (sin "s"). Es fácil escribir la URL equivocada por hábito.

### Cómo quedó configurado el push dual

Cada uno de esos tres repos tiene:

- `origin` → fetch de GitLab, push a **GitLab y GitHub** (dos líneas `push` en `git remote -v`).
- `github` → remote de solo lectura hacia GitHub, para poder hacer `git fetch github` y traer
  commits que estén únicamente ahí.

```bash
git remote -v
# origin  git@gitlab.com:...  (fetch)
# origin  git@gitlab.com:...  (push)
# origin  git@github.com:...  (push)
# github  git@github.com:...  (fetch)
# github  git@github.com:...  (push)
```

Consecuencias a tener presentes:

- `git pull` solo trae de GitLab (la URL de fetch de `origin`). Para traer de GitHub:
  `git fetch github`.
- `git push --force` impacta **ambos** remotos a la vez.
- Si el push a un remoto falla y al otro no, `origin` queda con las dos plataformas
  desincronizadas — conviene leer la salida completa del push, no solo el código de retorno.

### Historial de la migración (2026-09-15)

Al montar este espejo se encontraron dos cosas que ya quedaron resueltas y no deberían volver
a aparecer, pero se documentan por si generan dudas al ver el historial:

- **`main` de GitHub iba adelante del de GitLab** en `portfolio` (+5 commits) y `dashboard`
  (+4 commits). Eran fast-forwards limpios (GitLab era ancestro directo), así que se alinearon
  antes de configurar el push dual — no se perdió ni se sobrescribió nada.
- **Existían dos repos `backend` en GitHub**: uno Python de 2021 en `chaoticteam/backend`
  (ya renombrado a `chaoticteam/backend-old`) y el Java real bajo `Al3xDiaz/backend` (ya
  transferido a `chaoticteam/backend`). El backend válido es el Java; `backend-old` es historia
  muerta que se conserva por si hace falta consultarla.

## Otras carpetas

- `mobile/fit-swap` y `utils/gitversion-template` no siguen el patrón de push dual — son repos
  normales, cada uno con su propio origin.
