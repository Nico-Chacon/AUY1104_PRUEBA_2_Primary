# AUY1104_PRUEBA_2_Primary — Plantilla reutilizable de CI/CD

Este repositorio contiene la "receta compartida" (`workflow_call`) usada por
los microservicios del curso (ej. `TechMarket Orders`) para estandarizar su
pipeline de build, push y despliegue Blue-Green en Kubernetes.

## Contenido

- `.github/workflows/deploy-api.yaml`: plantilla reutilizable con las
  etapas de test, build & push de imagen Docker, despliegue Blue-Green,
  validación de salud y rollback automático. Ver comentarios inline en el
  archivo para el detalle de cada etapa.
- `.github/workflows/ea2-lab-dispatch-main.yaml`: workflow de utilidad para
  aprovisionar el entorno de laboratorio (EC2 + K3s/minikube) en AWS
  Academy, delegado a su vez a un workflow compartido del curso. No forma
  parte del pipeline de la aplicación; se usa una sola vez para levantar el
  clúster antes de desplegar.

## Cómo se invoca desde un repo cliente

```yaml
jobs:
  usar-receta-compartida:
    uses: Nico-Chacon/AUY1104_PRUEBA_2_Primary/.github/workflows/deploy-api.yaml@main
    with:
      image-name: orders
      image-tag: ${{ github.ref_name }}
      k3s-server-public-ip: ${{ vars.K3S_SERVER_PUBLIC_IP }}
    secrets:
      DOCKER_USERNAME: ${{ secrets.DOCKER_USERNAME }}
      DOCKER_PASSWORD: ${{ secrets.DOCKER_PASSWORD }}
      EA2_SSH_PRIVATE_KEY: ${{ secrets.EA2_SSH_PRIVATE_KEY }}
```

El repo cliente debe incluir su propia carpeta `k8s/` con:
`deployment-blue.yaml`, `deployment-green.yaml`, `service.yaml`,
`watchdog-rbac.yaml` y `watchdog-deployment.yaml` (ver repo `Orders-App`
como referencia).

![meow](meow.jpg)