# Инструкция по установке ПО киоска Trovemat
Возможна установка следующих вариантов ПО киоска:
1. [Linux Mint Cinnamon x64](https://linuxmint.com/edition.php?id=237) - версия для тестирования и демонстрации, содержит рабочий стол и легко управляется пользователями с минимальным опытом (или вообще без опыта) работы с операционной системой Linux. Рекомендуется использовать данный вариант, если требуется ознакомительная установка ПО Trovemat.
1. [Ubuntu 16.04 Server x64](http://releases.ubuntu.com/16.04/ubuntu-16.04.3-server-amd64.iso) - версия для использования в промышленной эксплуатации. Содержит все настройки, необходимые для защищённой работы ПО Trovemat.

## Инструкция по установке ПО киоска Trovemat на ОС Linux Mint Cinnamon x64
1. Установка Trovemat производится на операционной системе Linux Mint Cinnamon x64.
1. Установить Trovemat (ввести команду в терминале)  
    
    > !!! ВНИМАНИЕ  
    > !!! ДАННАЯ КОМАНДА УСТАНАВЛИВАЕТ ДЕМОНСТРАЦИОННУЮ ВЕРСИЮ ПО КИОСКА  
    > !!! ДЛЯ ПОЛУЧЕНИЯ ЛИЦЕНЗИОННОЙ ВЕРСИИ БЕЗ ОГРАНИЧЕНИЙ  
    > !!! ОБРАЩАЙТЕСЬ НА sales@trovemat.com  
  
    ``` SHELL 
    wget https://trovemat.com/trovemat_online_installer.sh -O /tmp/trovemat_online_installer.sh && sudo bash /tmp/trovemat_online_installer.sh $USER
    ```
1. Откалибровать сенсорный дисплей: в терминале набрать команду:
    ``` SHELL 
    sudo apt install xinput-calibrator && xinput_calibrator --device {id}
    ```
    id Тачскрина (вводится без фигурных скобок) можно посмотреть после установки Trovemat в списке устройств.
1. Перезагрузить систему, > Trovemat запустится автоматически при загрузке системы.
1. Настроить ПО Trovemat в соответствии с документом ["Инструкция по использованию ПО киоска Trovemat"](https://github.com/trovemat/docs/blob/master/Kiosk/User%20Guide%20%5Bru%5D.md)

## Инструкция по установке ПО киоска Trovemat на ОС Ubuntu 16.04 Desktop x64
1. Установка Trovemat производится на операционной системе Ubuntu 16.04 Desktop x64
    1. Убедитесь в процессе установки что выбрана опция установки "OpenSSH Server".
    1. Установка ОС производится с настройками по умолчанию.
1. Установить Trovemat (ввести команду в терминале)  
    
    > !!! ВНИМАНИЕ  
    > !!! ДАННАЯ КОМАНДА УСТАНАВЛИВАЕТ ДЕМОНСТРАЦИОННУЮ ВЕРСИЮ ПО КИОСКА  
    > !!! ДЛЯ ПОЛУЧЕНИЯ ЛИЦЕНЗИОННОЙ ВЕРСИИ БЕЗ ОГРАНИЧЕНИЙ  
    > !!! ОБРАЩАЙТЕСЬ НА sales@trovemat.com  
  
    ``` SHELL 
    wget https://trovemat.com/trovemat_online_installer_ubuntu.sh
    chmod +x trovemat_online_installer_ubuntu.sh
    sudo ./trovemat_online_installer_ubuntu.sh $USER
    ```
1. По завершении установки Trovemat компьютер будет перезапущен. После перезагрузки системы Trovemat запустится автоматически.
1. Настроить ПО Trovemat в соответствии с документом ["Инструкция по использованию ПО киоска Trovemat"](https://github.com/trovemat/docs/blob/master/Kiosk/User%20Guide%20%5Bru%5D.md)
