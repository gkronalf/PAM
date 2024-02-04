# Пользователи и группы. Авторизация и аутентификация 
### Цель    
Научиться создавать пользователей и добавлять им ограничения  
  
### Описание    
  - Запретить всем пользователям, кроме группы admin, логин в выходные (суббота и воскресенье), без учета праздников  
  
  Для выполения цели, создан стенд vagrant. Созданы пользователи otus, c паролем Otus2024! и пользователь otusadm , c паролем Otus2024! ```echo "Otus2024!" | sudo passwd --stdin otusadm && echo "Otus2024!" | sudo passwd --stdin otus ``````
- Созданна группа admin ``` groupadd admin ```, в которую добавлен пользователь otusadm;  
- Создадан файл-скрипт login.sh, который размещен в /usr/local/bin/login.sh и добавлены права на запуск скрипта ``` chmod +x /usr/local/bin/login.sh ```;  
- В файле /etc/pam.d/sshd указан модуль pam_exec и наш скрипт /usr/local/bin/login.sh. ``` echo "auth       required     pam_exec.so /usr/local/bin/login.sh" >> /etc/pam.d/sshd ```


