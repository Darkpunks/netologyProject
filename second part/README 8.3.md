__________________________________________________________________________
Домашнее задание к занятию "8.4 Создание собственных modules"
__________________________________________________________________________

Наша основная цель - разбить наш playbook на отдельные roles. Задача: сделать roles для elastic, kibana и написать playbook для использования этих ролей. Ожидаемый результат: существуют два ваших репозитория с roles и один репозиторий с playbook.

1. Создать в старой версии playbook файл requirements.yml и заполнить его следующим содержимым:
---
  - src: git@github.com:netology-code/mnt-homeworks-ansible.git
    scm: git
    version: "1.0.1"
    name: java 



2. При помощи ansible-galaxy скачать себе эту роль. Запустите molecule test, посмотрите на вывод команды.


```
ivan@ubt-tst:~/playbook-ansible-netology$ ansible-galaxy install -r requirements.yml --roles-path ./roles
Starting galaxy role install process
- extracting java to /home/ivan/playbook-ansible-netology/roles/java
- java (1.0.1) was installed successfully
```


molecule test 

```
ivan@ubt-tst:~/playbook-ansible-netology/roles/java$ python3 -m molecule test
INFO     default scenario test matrix: dependency, lint, cleanup, destroy, syntax, create, prepare, converge, idempotence, side_effect, verify, cleanup, destroy
INFO     Performing prerun with role_name_check=0...
INFO     Set ANSIBLE_LIBRARY=/home/ivan/.cache/ansible-compat/38a096/modules:/home/ivan/.ansible/plugins/modules:/usr/share/ansible/plugins/modules
INFO     Set ANSIBLE_COLLECTIONS_PATH=/home/ivan/.cache/ansible-compat/38a096/collections:/home/ivan/.ansible/collections:/usr/share/ansible/collections
INFO     Set ANSIBLE_ROLES_PATH=/home/ivan/.cache/ansible-compat/38a096/roles:/home/ivan/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles
ERROR    Computed fully qualified role name of java does not follow current galaxy requirements.
Please edit meta/main.yml and assure we can correctly determine full role name:

galaxy_info:
role_name: my_name  # if absent directory name hosting role is used instead
namespace: my_galaxy_namespace  # if absent, author is used instead

Namespace: https://galaxy.ansible.com/docs/contributing/namespaces.html#galaxy-namespace-limitations
Role: https://galaxy.ansible.com/docs/contributing/creating_role.html#role-names

As an alternative, you can add 'role-name' to either skip_list or warn_list.

Traceback (most recent call last):
  File "/usr/lib/python3.8/runpy.py", line 194, in _run_module_as_main
    return _run_code(code, main_globals, None,
  File "/usr/lib/python3.8/runpy.py", line 87, in _run_code
    exec(code, run_globals)
  File "/home/ivan/.local/lib/python3.8/site-packages/molecule/__main__.py", line 25, in <module>
    main()
  File "/home/ivan/.local/lib/python3.8/site-packages/click/core.py", line 1130, in __call__
    return self.main(*args, **kwargs)
  File "/home/ivan/.local/lib/python3.8/site-packages/click/core.py", line 1055, in main
    rv = self.invoke(ctx)
  File "/home/ivan/.local/lib/python3.8/site-packages/click/core.py", line 1657, in invoke
    return _process_result(sub_ctx.command.invoke(sub_ctx))
  File "/home/ivan/.local/lib/python3.8/site-packages/click/core.py", line 1404, in invoke
    return ctx.invoke(self.callback, **ctx.params)
  File "/home/ivan/.local/lib/python3.8/site-packages/click/core.py", line 760, in invoke
    return __callback(*args, **kwargs)
  File "/home/ivan/.local/lib/python3.8/site-packages/click/decorators.py", line 26, in new_func
    return f(get_current_context(), *args, **kwargs)
  File "/home/ivan/.local/lib/python3.8/site-packages/molecule/command/test.py", line 176, in test
    base.execute_cmdline_scenarios(scenario_name, args, command_args, ansible_args)
  File "/home/ivan/.local/lib/python3.8/site-packages/molecule/command/base.py", line 112, in execute_cmdline_scenarios
    scenario.config.runtime.prepare_environment(
  File "/home/ivan/.local/lib/python3.8/site-packages/ansible_compat/runtime.py", line 362, in prepare_environment
    self._install_galaxy_role(
  File "/home/ivan/.local/lib/python3.8/site-packages/ansible_compat/runtime.py", line 522, in _install_galaxy_role
    raise InvalidPrerequisiteError(msg)
ansible_compat.errors.InvalidPrerequisiteError: Computed fully qualified role name of java does not follow current galaxy requirements.
Please edit meta/main.yml and assure we can correctly determine full role name:

galaxy_info:
role_name: my_name  # if absent directory name hosting role is used instead
namespace: my_galaxy_namespace  # if absent, author is used instead

Namespace: https://galaxy.ansible.com/docs/contributing/namespaces.html#galaxy-namespace-limitations
Role: https://galaxy.ansible.com/docs/contributing/creating_role.html#role-names

As an alternative, you can add 'role-name' to either skip_list or warn_list.
```

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/8.3/8.3.2.jpg">

3. Перейдите в каталог с ролью elastic-role и создайте сценарий тестирования по умолчаню при помощи molecule init scenario --driver-name docker.

```
ivan@ubt-tst:~/netology-elastic-role$ python3 -m molecule init scenario -d docker
INFO     Initializing new scenario default...
INFO     Initialized scenario in /home/ivan/netology-elastic-role/molecule/default successfully.
ivan@ubt-tst:~/netology_elastic_role$ python3 -m molecule init role netology_elastic_role.netology
INFO     Initializing new role netology...
Using /etc/ansible/ansible.cfg as config file
- Role netology was created successfully
Invalid -W option ignored: unknown warning category: 'CryptographyDeprecationWarning'
Invalid -W option ignored: unknown warning category: 'CryptographyDeprecationWarning'
localhost | CHANGED => {"backup": "","changed": true,"msg": "line added"}
INFO     Initialized role in /home/ivan/netology_elastic_role/netology successfully.

```
 <img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/8.3/8.3.3.jpg">
 
 
 

4. Добавьте несколько разных дистрибутивов (centos:8, ubuntu:latest) для инстансов и протестируйте роль, исправьте найденные ошибки, если они есть.

Тест выдал ошибку fatal: [instance]: FAILED! => {"attempts": 3, "changed": false, "dest": "/tmp/elasticsearch-7.10.1-linux-x86_64.tar.gz", "elapsed": 0, "msg": "Request failed", "response": "HTTP Error 403: Forbidden", "status_code": 403, "url": "https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.10.1-linux-x86_64.tar.gz"}

Исправил ошибkу
```
ivan@ubt-tst:~/netology_elastic_role$ python3 -m molecule test
INFO     default scenario test matrix: dependency, lint, cleanup, destroy, syntax, create, prepare, converge, idempotence, side_effect, verify, cleanup, destroy
INFO     Performing prerun with role_name_check=0...
INFO     Set ANSIBLE_LIBRARY=/home/ivan/.cache/ansible-compat/d864d4/modules:/home/ivan/.ansible/plugins/modules:/usr/share/ansible/plugins/modules
INFO     Set ANSIBLE_COLLECTIONS_PATH=/home/ivan/.cache/ansible-compat/d864d4/collections:/home/ivan/.ansible/collections:/usr/share/ansible/collections
INFO     Set ANSIBLE_ROLES_PATH=/home/ivan/.cache/ansible-compat/d864d4/roles:/home/ivan/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles
INFO     Using /home/ivan/.cache/ansible-compat/d864d4/roles/netology.netology_elastic_role symlink to current repository in order to enable Ansible to find the role using its expected full name.
INFO     Running default > dependency
WARNING  Skipping, missing the requirements file.
WARNING  Skipping, missing the requirements file.
INFO     Running default > lint
INFO     Lint is disabled.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy
INFO     Sanity checks: 'docker'

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=instance_centos8)
changed: [localhost] => (item=ubuntu)

TASK [Wait for instance(s) deletion to complete] *******************************
ok: [localhost] => (item=instance_centos8)
ok: [localhost] => (item=ubuntu)

TASK [Delete docker networks(s)] ***********************************************

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Running default > syntax

playbook: /home/ivan/netology_elastic_role/molecule/default/converge.yml
INFO     Running default > create

PLAY [Create] ******************************************************************

TASK [Log into a Docker registry] **********************************************
skipping: [localhost] => (item=None)
skipping: [localhost] => (item=None)
skipping: [localhost]

TASK [Check presence of custom Dockerfiles] ************************************
ok: [localhost] => (item={'image': 'quay.io/centos/centos:stream8', 'name': 'instance_centos8', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})

TASK [Create Dockerfiles from image names] *************************************
skipping: [localhost] => (item={'image': 'quay.io/centos/centos:stream8', 'name': 'instance_centos8', 'pre_build_image': True})
skipping: [localhost] => (item={'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})

TASK [Discover local Docker images] ********************************************
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'image': 'quay.io/centos/centos:stream8', 'name': 'instance_centos8', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 0, 'ansible_index_var': 'i'})
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 1, 'ansible_index_var': 'i'})

TASK [Build an Ansible compatible image (new)] *********************************
skipping: [localhost] => (item=molecule_local/quay.io/centos/centos:stream8)
skipping: [localhost] => (item=molecule_local/pycontribs/ubuntu:latest)

TASK [Create docker network(s)] ************************************************

TASK [Determine the CMD directives] ********************************************
ok: [localhost] => (item={'image': 'quay.io/centos/centos:stream8', 'name': 'instance_centos8', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})

TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item=instance_centos8)
changed: [localhost] => (item=ubuntu)

TASK [Wait for instance(s) creation to complete] *******************************
changed: [localhost] => (item={'failed': 0, 'started': 1, 'finished': 0, 'ansible_job_id': '15690607712.85229', 'results_file': '/home/ivan/.ansible_async/15690607712.85229', 'changed': True, 'item': {'image': 'quay.io/centos/centos:stream8', 'name': 'instance_centos8', 'pre_build_image': True}, 'ansible_loop_var': 'item'})
FAILED - RETRYING: [localhost]: Wait for instance(s) creation to complete (300 retries left).
changed: [localhost] => (item={'failed': 0, 'started': 1, 'finished': 0, 'ansible_job_id': '679814723444.85257', 'results_file': '/home/ivan/.ansible_async/679814723444.85257', 'changed': True, 'item': {'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost                  : ok=5    changed=2    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0

INFO     Running default > prepare
WARNING  Skipping, prepare playbook not configured.
INFO     Running default > converge

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [instance_centos8]
ok: [ubuntu]

TASK [Include netology_elastic_role] *******************************************

TASK [netology_elastic_role : Upload tar.gz Elasticsearch from local storage] ***
changed: [instance_centos8]
changed: [ubuntu]

TASK [netology_elastic_role : Create directrory for Elasticsearch] *************
changed: [ubuntu]
changed: [instance_centos8]

TASK [netology_elastic_role : Extract Elasticsearch in the installation directory] ***
changed: [instance_centos8]
changed: [ubuntu]

TASK [netology_elastic_role : Set environment Elastic] *************************
changed: [ubuntu]
changed: [instance_centos8]

PLAY RECAP *********************************************************************
instance_centos8           : ok=5    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=5    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Running default > idempotence

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [instance_centos8]
ok: [ubuntu]

TASK [Include netology_elastic_role] *******************************************

TASK [netology_elastic_role : Upload tar.gz Elasticsearch from local storage] ***
ok: [ubuntu]
ok: [instance_centos8]

TASK [netology_elastic_role : Create directrory for Elasticsearch] *************
ok: [instance_centos8]
ok: [ubuntu]

TASK [netology_elastic_role : Extract Elasticsearch in the installation directory] ***
skipping: [ubuntu]
skipping: [instance_centos8]

TASK [netology_elastic_role : Set environment Elastic] *************************
ok: [instance_centos8]
ok: [ubuntu]

PLAY RECAP *********************************************************************
instance_centos8           : ok=4    changed=0    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
ubuntu                     : ok=4    changed=0    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Idempotence completed successfully.
INFO     Running default > side_effect
WARNING  Skipping, side effect playbook not configured.
INFO     Running default > verify
INFO     Running Ansible Verifier

PLAY [Verify] ******************************************************************

TASK [Example assertion] *******************************************************
ok: [instance_centos8] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [ubuntu] => {
    "changed": false,
    "msg": "All assertions passed"
}

PLAY RECAP *********************************************************************
instance_centos8           : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Verifier completed successfully.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=instance_centos8)
changed: [localhost] => (item=ubuntu)

TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: [localhost]: Wait for instance(s) deletion to complete (300 retries left).
changed: [localhost] => (item=instance_centos8)
changed: [localhost] => (item=ubuntu)

TASK [Delete docker networks(s)] ***********************************************

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Pruning extra files from scenario ephemeral directory
```

5. Создайте новый каталог с ролью при помощи molecule init role --driver-name docker kibana-role. Можете использовать другой драйвер, который более удобен вам.
```
ivan@ubt-tst:~/playbook_ansible_netology/roles$ python3 -m molecule init role netology.kibana_role --driver-name docker
INFO     Initializing new role kibana_role...
Using /etc/ansible/ansible.cfg as config file
- Role kibana_role was created successfully
Invalid -W option ignored: unknown warning category: 'CryptographyDeprecationWarning'
Invalid -W option ignored: unknown warning category: 'CryptographyDeprecationWarning'
localhost | CHANGED => {"backup": "","changed": true,"msg": "line added"}
INFO     Initialized role in /home/ivan/playbook_ansible_netology/roles/kibana_role successfully.
```


6. На основе tasks из старого playbook заполните новую role. Разнесите переменные между vars и default. Проведите тестирование на разных дистрибитивах (centos:7, centos:8, ubuntu).

```
ivan@ubt-tst:~/playbook_ansible_netology/roles/kibana_role$ python3 -m molecule test
INFO     default scenario test matrix: dependency, lint, cleanup, destroy, syntax, create, prepare, converge, idempotence, side_effect, verify, cleanup, destroy
INFO     Performing prerun with role_name_check=0...
INFO     Set ANSIBLE_LIBRARY=/home/ivan/.cache/ansible-compat/002ddc/modules:/home/ivan/.ansible/plugins/modules:/usr/share/ansible/plugins/modules
INFO     Set ANSIBLE_COLLECTIONS_PATH=/home/ivan/.cache/ansible-compat/002ddc/collections:/home/ivan/.ansible/collections:/usr/share/ansible/collections
INFO     Set ANSIBLE_ROLES_PATH=/home/ivan/.cache/ansible-compat/002ddc/roles:/home/ivan/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles
INFO     Using /home/ivan/.cache/ansible-compat/002ddc/roles/netology.kibana_role symlink to current repository in order to enable Ansible to find the role using its expected full name.
INFO     Running default > dependency
INFO     Running ansible-galaxy collection install -v --pre community.docker:>=3.0.0-a2
WARNING  Skipping, missing the requirements file.
WARNING  Skipping, missing the requirements file.
INFO     Running default > lint
INFO     Lint is disabled.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy
INFO     Sanity checks: 'docker'

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=instance_centos8)
changed: [localhost] => (item=ubuntu)

TASK [Wait for instance(s) deletion to complete] *******************************
ok: [localhost] => (item=instance_centos8)
ok: [localhost] => (item=ubuntu)

TASK [Delete docker networks(s)] ***********************************************

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Running default > syntax

playbook: /home/ivan/playbook_ansible_netology/roles/kibana_role/molecule/default/converge.yml
INFO     Running default > create

PLAY [Create] ******************************************************************

TASK [Log into a Docker registry] **********************************************
skipping: [localhost] => (item=None)
skipping: [localhost] => (item=None)
skipping: [localhost]

TASK [Check presence of custom Dockerfiles] ************************************
ok: [localhost] => (item={'image': 'quay.io/centos/centos:stream8', 'name': 'instance_centos8', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})

TASK [Create Dockerfiles from image names] *************************************
skipping: [localhost] => (item={'image': 'quay.io/centos/centos:stream8', 'name': 'instance_centos8', 'pre_build_image': True})
skipping: [localhost] => (item={'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})

TASK [Discover local Docker images] ********************************************
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'image': 'quay.io/centos/centos:stream8', 'name': 'instance_centos8', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 0, 'ansible_index_var': 'i'})
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 1, 'ansible_index_var': 'i'})

TASK [Build an Ansible compatible image (new)] *********************************
skipping: [localhost] => (item=molecule_local/quay.io/centos/centos:stream8)
skipping: [localhost] => (item=molecule_local/pycontribs/ubuntu:latest)

TASK [Create docker network(s)] ************************************************

TASK [Determine the CMD directives] ********************************************
ok: [localhost] => (item={'image': 'quay.io/centos/centos:stream8', 'name': 'instance_centos8', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})

TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item=instance_centos8)
changed: [localhost] => (item=ubuntu)

TASK [Wait for instance(s) creation to complete] *******************************
changed: [localhost] => (item={'failed': 0, 'started': 1, 'finished': 0, 'ansible_job_id': '764753678110.103184', 'results_file': '/home/ivan/.ansible_async/764753678110.103184', 'changed': True, 'item': {'image': 'quay.io/centos/centos:stream8', 'name': 'instance_centos8', 'pre_build_image': True}, 'ansible_loop_var': 'item'})
FAILED - RETRYING: [localhost]: Wait for instance(s) creation to complete (300 retries left).
changed: [localhost] => (item={'failed': 0, 'started': 1, 'finished': 0, 'ansible_job_id': '620578752331.103212', 'results_file': '/home/ivan/.ansible_async/620578752331.103212', 'changed': True, 'item': {'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost                  : ok=5    changed=2    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0

INFO     Running default > prepare
WARNING  Skipping, prepare playbook not configured.
INFO     Running default > converge

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [instance_centos8]
ok: [ubuntu]

TASK [Include netology.kibana_role] ********************************************

TASK [netology.kibana_role : Upload tar.gz kibana from local storage] **********
changed: [instance_centos8]
changed: [ubuntu]

TASK [netology.kibana_role : Create directrory for kibana] *********************
changed: [ubuntu]
changed: [instance_centos8]

TASK [netology.kibana_role : Extract Kibana in the installation directory] *****
changed: [instance_centos8]
changed: [ubuntu]

TASK [netology.kibana_role : Set environment Kibana] ***************************
changed: [ubuntu]
changed: [instance_centos8]

PLAY RECAP *********************************************************************
instance_centos8           : ok=5    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=5    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Running default > idempotence

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [instance_centos8]
ok: [ubuntu]

TASK [Include netology.kibana_role] ********************************************

TASK [netology.kibana_role : Upload tar.gz kibana from local storage] **********
ok: [instance_centos8]
ok: [ubuntu]

TASK [netology.kibana_role : Create directrory for kibana] *********************
ok: [instance_centos8]
ok: [ubuntu]

TASK [netology.kibana_role : Extract Kibana in the installation directory] *****
skipping: [instance_centos8]
skipping: [ubuntu]

TASK [netology.kibana_role : Set environment Kibana] ***************************
ok: [instance_centos8]
ok: [ubuntu]

PLAY RECAP *********************************************************************
instance_centos8           : ok=4    changed=0    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
ubuntu                     : ok=4    changed=0    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Idempotence completed successfully.
INFO     Running default > side_effect
WARNING  Skipping, side effect playbook not configured.
INFO     Running default > verify
INFO     Running Ansible Verifier

PLAY [Verify] ******************************************************************

TASK [Example assertion] *******************************************************
ok: [instance_centos8] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [ubuntu] => {
    "changed": false,
    "msg": "All assertions passed"
}

PLAY RECAP *********************************************************************
instance_centos8           : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Verifier completed successfully.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=instance_centos8)
changed: [localhost] => (item=ubuntu)

TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: [localhost]: Wait for instance(s) deletion to complete (300 retries left).
changed: [localhost] => (item=instance_centos8)
changed: [localhost] => (item=ubuntu)

TASK [Delete docker networks(s)] ***********************************************

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Pruning extra files from scenario ephemeral directory
```

7. Выложите все roles в репозитории.

https://github.com/Darkpunks/netology_kibana_role

https://github.com/Darkpunks/netology_elastic_role

8. Добавьте roles в requirements.yml в playbook.

добавил

9. Переработайте playbook на использование roles.

исправил

10.Выложите playbook в репозиторий.

https://github.com/Darkpunks/playbook_ansible_netology

11.В ответ приведите ссылки на оба репозитория с roles и одну ссылку на репозиторий с playbook.

Сделано. 
