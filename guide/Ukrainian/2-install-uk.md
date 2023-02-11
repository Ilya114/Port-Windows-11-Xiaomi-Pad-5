# Встановлення Windows

### Завантажте відновлення, щоб розпочати встановлення Windows

```cmd
fastboot boot <recovery.img>
```

### Виконайте сценарій msc.sh

```cmd
adb shell sh msc.sh
```

## Призначте літери розділам

### Запуск diskpart

> Коли ваш Xiaomi Pad 5 визначився як диск

```cmd
diskpart
```

### Призначення літери `X` розділу Windows

#### Вибір розділа Windows у планшеті
> Використовуйте `list volume` для того, щоб знайти розділ Windows, він називається "WINNABU"

```diskpart
select volume <номер>
```

#### Призначення літери `X`
```diskpart
assign letter=x
```

### Призначення літери `Y` розділу ESP

#### Вибір розділа ESP в телефоні
> Використовуйте `list volume` для того, щоб знайти розділ ESP, він назвається "ESPNABU"

```diskpart
select volume <номер>
```

### Призначення літери `Y`

```diskpart
assign letter=y
```

### Вихід із diskpart:
```diskpart
exit
```


## Встановлення

> Замініть `<path/to/install.wim>` дійсним шляхом до install.wim,

> `install.wim` знаходиться в теці sources всередині вашого .iso

> Ви можете отримати цей файл розпакувавши або смонтувавши його

```cmd
dism /apply-image /ImageFile:<path/to/install.wim> /index:1 /ApplyDir:X:\
```

# Встановлення драйверів

> Замініть `<nabudriversfolder>` шляхом к теці с вашими драйверами

```cmd
driverupdater.exe -d <nabudriversfolder>\definitions\Desktop\ARM64\Internal\nabu.txt -r <nabudriversfolder> -p X:
```

# Створіть файли завантажувача Windows

```cmd
bcdboot X:\Windows /s Y: /f UEFI
```

# Дозвольте непідписані драйвера

> Якщо ви цього не зробите, то отримаєте BSOD

```cmd
bcdedit /store Y:\EFI\Microsoft\BOOT\BCD /set {default} testsigning on
```

# Завантажтеся у Windows

### Зробіть резервну копію поточного загрузочного розділа

```cmd
adb shell "dd if=/dev/block/bootdevice/by-name/boot$(getprop ro.boot.slot_suffix) of=/tmp/boot.img"
```

##### Скопіюйте резервну копію на компьютер

```cmd
adb pull /tmp/boot.img
```

### Завантажте відновлення

```cmd
adb reboot bootloader
```

### Прошийте образ uefi з TWRP

```cmd
fastboot flash boot boot-nabu.img
```

# Завантажте Android
> Використовуйте резервну копію завантажувального образу та прошийте з швидкого завантаження

```cmd
fastboot flash boot boot.img
```

# Готово!
