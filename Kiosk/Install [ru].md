# Инструкция по установке ПО киоска Trovemat
1. Установка Trovemat производится на операционной системе  Linux Mint Cinnamon x64.
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
1. Настроить файлы config.xml, default.chq, menu.xml, operators.xml (см. документ "Инструкция по использованию ПО киоска Trovemat").
1. Перезагрузить систему, > Trovemat запустится автоматически при загрузке системы.
