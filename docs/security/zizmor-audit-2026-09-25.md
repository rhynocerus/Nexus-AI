# Informe inicial de zizmor

- Repositorio: `rhynocerus/Nexus-AI`
- Fecha: 2026-09-25
- Commit analizado: `d6c0793`
- Herramienta: zizmor 1.30.1
- Alcance: `.github/workflows/`
- Modo: análisis local offline
- Cambios automáticos aplicados: ninguno

## Resumen

zizmor informó 12 hallazgos visibles (8 de nivel alto y 4 de nivel medio) y 7 suprimidos: 19 en total. El análisis offline no ejecuta algunas auditorías que requieren acceso a GitHub.

## Hallazgos visibles

### Referencias a Actions sin fijar a SHA

Se señalaron ocho usos con etiquetas de versión en vez de un SHA completo:

- `android-build.yml`: `actions/checkout@v4`, `actions/setup-java@v4`, `actions/cache@v4` y `actions/upload-artifact@v4`.
- `autofix.yml`: `actions/checkout@v4`, `actions/setup-java@v4`, `gradle/actions/setup-gradle@v3` y `peter-evans/create-pull-request@v6`.

El resultado señala que estas referencias no están fijadas a un SHA completo. Conviene valorar esta medida para cada dependencia, verificar el SHA y conservar un comentario con la versión legible para facilitar futuras actualizaciones.

### Permisos no declarados

zizmor marcó los dos jobs por depender de los permisos predeterminados del repositorio:

- `android-build.yml`: job `build`.
- `autofix.yml`: job `fix-and-pr`.

Se recomienda declarar permisos mínimos por job. Para la compilación, revisar si basta con `contents: read`. Para el workflow de formato, confirmar los permisos mínimos necesarios para que la acción cree commits y pull requests.

### Credenciales persistentes en checkout

En ambos workflows, `actions/checkout` no establece `persist-credentials: false`. Conviene revisar si el checkout necesita conservar credenciales durante los pasos posteriores y desactivarlo cuando no sea necesario.

## Limitaciones

El análisis se ejecutó en modo offline. Por tanto, algunas auditorías que consultan GitHub o resuelven dependencias remotas no estuvieron disponibles. Los resultados son una revisión inicial, no una certificación de seguridad.

## Siguiente revisión

Revisar cada hallazgo contra el propósito de los workflows antes de modificar permisos o dependencias. Ejecutar zizmor de nuevo después de cualquier cambio y conservar el resultado actualizado.
