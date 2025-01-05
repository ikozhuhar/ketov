<p align="center">
<img src="https://github.com/ikozhuhar/ketov/blob/main/img/ketov-linux.png">
</p>

<details>
<p><summary><b> :large_blue_diamond: Глава 1. Архитектура ОС Linux</b></summary></p>

В Линукс два главных режима работы: `kernel space` и `user space`. Главное отличие в режимах работы состоит в привилегиях доступа к аппаратных ресурсам. 

#### _Компоненты User Space_

**Kernel space** обеспечивает распределение ресурсов между пользователями и предоставляет базовый интерфейс для доступ к ресурсам.

Функции ядра доступны в **user mode** с помощью системных вызовов. **Системные вызовы** выполняются в ядре, а вызывается из **user space** с помощью библиотеки **libc.so**. 

Функции выполняющиеся в **user space** доступны с помощью библиотечных вызовов и выполняются в самих библиотеках, например, **libz.so** и  **libbz2.so**

#### _Компоненты Ядра_

Компоненты Ядра в основном обеспечивают распределение ресурсов, что приводит к появлению **менеджеров** _процессов_, _памяти_, _ввода-вывода_ и _менеджера файловой системы_.

- **Менеджер процессов** распределяет время ЦП между выполняющимися задачами.

- **Менеджер ввода-вывода** распределяет доступ к устройствам ввода-вывода между процессоми.

- **Менеджер памяти** распределяет пространство ОЗУ между процессами.

- **Файловый Менеджер** предоставляет процессам интерфейс файлового доступа к дискам (hdd). **Особое значение** менеджера файлов состоит в том, что с помощью файлового интерфейса процессам предоставляется доступ к другим подсистемам. Например, доступ к CD/DVD-накопителя через `/dev/sr0`, к мыши через `/dev/input/mouse`. Доступ к физ памяти через /dev/mem, доступ процессов к страницам памяти друг друга через `/proc/PID/mem`, а доступ к обнаруженным Ядром устройств через псевдофайловую систему `sysfs` каталога `/sys`.

Кроме указанных задач все менеджеры в совокупности предоставляют процессам средства межпроцессорного взаимодействия, такие как **сигналы**, **каналы**, **сокеты** и **разделяемая память**.

#### _Аппаратный уровень_

**Аппаратный уровень**: Этот уровень состоит из всех периферийных устройств, таких как оперативная память, жесткий диск, процессор и т.д.

![image](https://github.com/user-attachments/assets/d8cda107-d0c9-4377-93d7-7f70d5fd13a3)
![image](https://github.com/user-attachments/assets/65ec448f-06e7-4233-a62c-4e3f927dbf5b)
![image](https://github.com/user-attachments/assets/9ec41b9e-aef4-48fb-b026-068bc4871735)




</details>



<details>
<p><summary><b> :large_blue_diamond: Глава 2. Пользовательское окружение ОС Linux</b></summary></p>

На персональных ПК, для взаимодействия с пользователем используется клавиатура, видео-адаптер и монитор, которые формируют консоль. Консоль используется драйвером виртуальных интерфейсов для организации нескольких физических терминалов.

Узнать имя текущего терминала (а точнее, имя спец файла устройства) можно командой `tty`, а список всех терминальных входов пользователей - команды `users`, `who`, `w`.

![image](https://github.com/user-attachments/assets/865566e4-7a8f-40fe-9126-f8bb68870b40)


</details>
