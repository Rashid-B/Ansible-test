# Домашнее задание к занятию "08.01 Введение в Ansible"

## Подготовка к выполнению
1. Установите ansible версии 2.10 или выше.
2. Создайте свой собственный публичный репозиторий на github с произвольным именем.
3. Скачайте [playbook](./playbook/) из репозитория с домашним заданием и перенесите его в свой репозиторий.

## Основная часть
1. Попробуйте запустить playbook на окружении из `test.yml`, зафиксируйте какое значение имеет факт `some_fact` для указанного хоста при выполнении playbook'a.

   ```
   /playbook/group_vars/all
   some_fact: 12 
   ```

2. Найдите файл с переменными (group_vars) в котором задаётся найденное в первом пункте значение и поменяйте его на 'all default fact'.

   ```
   /playbook/group_vars/all
   some_fact:all default fact
   ```

3. Воспользуйтесь подготовленным (используется `docker`) или создайте собственное окружение для проведения дальнейших испытаний.

4. Проведите запуск playbook на окружении из `prod.yml`. Зафиксируйте полученные значения `some_fact` для каждого из `managed host`.

   ```
   ok: [centos7] => {
       "msg": "el"
   }
   ok: [debian] => {
       "msg": "deb"
   }
   ```

5. Добавьте факты в `group_vars` каждой из групп хостов так, чтобы для `some_fact` получились следующие значения: для `deb` - 'deb default fact', для `el` - 'el default fact'.

6. Повторите запуск playbook на окружении `prod.yml`. Убедитесь, что выдаются корректные значения для всех хостов.

   ```
   ok: [centos7] => {
       "msg": "el default factl"
   }
   ok: [debian] => {
       "msg": "deb default fact"
   }
   ```

7. При помощи `ansible-vault` зашифруйте факты в `group_vars/deb` и `group_vars/el` с паролем `netology`.

   ```
   ansible-vault encrypt group_vars/el/examp.yml group_vars/deb/examp.yml
   ```

8. Запустите playbook на окружении `prod.yml`. При запуске `ansible` должен запросить у вас пароль. Убедитесь в работоспособности.

   ```
   ansible-playbook -i inventory/prod.yml site.yml --ask-vault-pass
   ```

9. Посмотрите при помощи `ansible-doc` список плагинов для подключения. Выберите подходящий для работы на `control node`.

   ```
   ansible-doc -t connection -l | grepp control
   ```

10. В `prod.yml` добавьте новую группу хостов с именем  `local`, в ней разместите localhost с необходимым типом подключения.

    

11. Запустите playbook на окружении `prod.yml`. При запуске `ansible` должен запросить у вас пароль. Убедитесь что факты `some_fact` для каждого из хостов определены из верных `group_vars`.

12. Заполните `README.md` ответами на вопросы. Сделайте `git push` в ветку `master`. В ответе отправьте ссылку на ваш открытый репозиторий с изменённым `playbook` и заполненным `README.md`.

## Необязательная часть

1. При помощи `ansible-vault` расшифруйте все зашифрованные файлы с переменными.

   ```
   ansible-vault decrypt group_vars/el/examp.yml group_vars/deb/examp.yml
   ```

2. Зашифруйте отдельное значение `PaSSw0rd` для переменной `some_fact` паролем `netology`. Добавьте полученное значение в `group_vars/all/exmp.yml`.

   ```
   ioi@ioi:~/DevOps/ansible/08-ansible-01-base/playbook$ ansible-vault encrypt_string 'PaSSw0rd' --name 'some_fact'
   New Vault password: 
   Confirm New Vault password: 
   some_fact: !vault |
             $ANSIBLE_VAULT;1.1;AES256
             64343333366363313430306138383931663538313566623466356465636134366565373461353431
             3232663139633233633034613863623033356463353264310a306264356234373436363834303433
             64306262666637623631393330316266393532613738643437666463353532373262613033356662
             3666376638326535660a336566643263383030366263653533643764333766653031323965306263
             6563
   Encryption successful
   ioi@ioi:~/DevOps/ansible/08-ansible-01-base/playbook$ ansible-playbook -i inventory/prod.yml site.yml --ask-vault-pass
   Vault password: 
   
   PLAY [Print os facts] ********************************************************************************************************************************
   
   TASK [Gathering Facts] *******************************************************************************************************************************
   ok: [localhost]
   ok: [debian]
   ok: [centos7]
   
   TASK [Print OS] **************************************************************************************************************************************
   ok: [centos7] => {
       "msg": "CentOS"
   }
   ok: [debian] => {
       "msg": "Debian"
   }
   ok: [localhost] => {
       "msg": "Ubuntu"
   }
   
   TASK [Print fact] ************************************************************************************************************************************
   ok: [centos7] => {
       "msg": "el default factl"
   }
   ok: [debian] => {
       "msg": "deb default fact"
   }
   ok: [localhost] => {
       "msg": "PaSSw0rd"
   }
   
   PLAY RECAP *******************************************************************************************************************************************
   centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
   debian                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
   localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
   ```

3. Запустите `playbook`, убедитесь, что для нужных хостов применился новый `fact`.

4. Добавьте новую группу хостов `fedora`, самостоятельно придумайте для неё переменную. В качестве образа можно использовать [этот](https://hub.docker.com/r/pycontribs/fedora).

5. Напишите скрипт на bash: автоматизируйте поднятие необходимых контейнеров, запуск ansible-playbook и остановку контейнеров.

6. Все изменения должны быть зафиксированы и отправлены в вашей личный репозиторий.

---

### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
