# AGENTS.md — глобальные правила

## Kubernetes

- opencode разрешено редактировать файлы с манифестами Kubernetes в репозитории (yaml/helm/kustomize) — но только файлы, не применяя их к кластеру.
- kubectl/flux/helm разрешены ТОЛЬКО read-only: `kubectl get/describe/logs/explain`, `flux get`, `helm list/history/status/get`, `git diff` и т.п.
- Любые операции записи в кластер запрещены: `kubectl apply/create/delete/patch/edit/replace/scale/set/rollout`, `flux apply/reconcile`, `helm install/upgrade/uninstall/rollback`.
- Если задача требует применить изменения к кластеру, opencode должен остановиться и предложить сделать это через GitOps-пайплайн или вручную.

## Зоны доступности

- «Мёртвая зона rc1b»: зона доступности `rc1b` недоступна.
- Восстановление в зоне `rc1b` недоступно — не предлагать и не использовать его.
- «Мёртвая зона rc1a»: зона доступности `ru-central1-a` (`rc1a`) целиком недоступна.
- Восстановление и планирование ёмкости в `rc1a` недоступны — не предлагать и не использовать их. Живая зона — `ru-central1-d` (`rc1d`).

## Git

- opencode ЗАПРЕЩЕНО коммитить (`git commit`), пушить (`git push`), создавать PR, выполнять `git rebase`, `git reset`, `git cherry-pick` или любые другие операции, изменяющие историю репозитория или отправляющие изменения удалённо.
- opencode может выполнять только read-only операции git: `git status`, `git diff`, `git log`, `git branch`, `git show` и т.п.
- Если задача требует коммита, пуша или создания PR, opencode должен отказать и предложить пользователю выполнить действие вручную.
