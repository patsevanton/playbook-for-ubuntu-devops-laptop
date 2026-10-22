# TODO-ansible-galaxy: коллекции и playbook'и Ansible — не ставим

Плейбук не устанавливает ничего из Ansible Galaxy: ни коллекций, ни ролей, ни
готовых playbook'ов — ни из списка ниже, ни любых других.

Не ставим, потому что это строительные блоки для чужих проектов, а не утилиты
для ноутбука: их место — в `requirements.yml` конкретного проекта с фиксацией
версий (`ansible-galaxy install -r requirements.yml`), а не в общем
`~/.ansible` на каждой машине.

Из Ansible плейбук ставит только `ansible-core`, `ansible-lint` и `molecule`
с `molecule-plugins[docker]`; коллекции, нужные молекуле, она доставит сама.
