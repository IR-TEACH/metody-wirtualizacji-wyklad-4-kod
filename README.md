# Stacja pomiarowa w Kubernetesie (W4)

Manifesty do Wykładu 4 z Metod wirtualizacji. Aplikacja ta sama co w W3 (`metody-wirtualizacji-wyklad-3-kod`); obrazy pochodzą z pipeline'u W3: `ghcr.io/ir-teach/stacja-<usługa>:<SHA>`.

| Plik | Co zawiera |
|---|---|
| `k8s/config.yaml` | ConfigMap `stacja` (ustawienia jawne) |
| `k8s/db.yaml` | PostgreSQL: PVC, Deployment, Service `db` |
| `k8s/backend.yaml` | Deployment (2 repliki) i Service `backend` |
| `k8s/frontend.yaml` | nginx ze stroną, Service `LoadBalancer` na `localhost:8080` |
| `k8s/sensor.yaml` | 3 czujniki = 3 sale |

Klaster: Kubernetes z Docker Desktop (kubeadm, jeden węzeł).

```bash
kubectl config use-context docker-desktop
cp .env.example .env                                   # hasło nie trafia do Gita
kubectl create secret generic stacja --from-env-file=.env
kubectl apply -f k8s/
kubectl get pods
```

Strona: http://localhost:8080, API: http://localhost:8080/api/docs

Etapy wykładu: `git checkout czesc-a` (wdrożenie bez sond), `git checkout czesc-b` (backend z sondami readiness i liveness, to samo co `main`).

Sprzątanie: `kubectl delete -f k8s/` (usuwa też PVC z danymi) i `kubectl delete secret stacja`.
