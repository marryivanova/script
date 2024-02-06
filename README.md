# script
Задание: Напишите скрипт на Python, который  будет запускать утилиту driverquery с сохранением результатов в файл, а потом открывать этот файл и выводить только драйвера с типом File System

Этот код представляет собой скрипт Python, который выполняет две функции: сначала запускает команду driverquery с помощью subprocess и сохраняет результат в файл, а затем фильтрует содержимое этого файла и выводит по драйверам, связанным с файловой системой.
Краткое описание реализации:

Истользовала: 
- import argparse: Импортирует модуль argparse, который позволяет создавать удобные интерфейсы командной.
- import subprocess: Импортирует модуль subprocess, который позволяет выполнять внешние команды.
­- import subprocess: Для работы с кодировками 

Концепция кода состоит в выполнении двух основных задач: запуск команды driverquery и сохранение ее вывода в файл, а затем фильтрация этого файла и вывод только драйверов, связанных с файловой систем.

­Реализация кода начинается с определения функции run_driverquery, которая принимает один аргумент output_file. Функция открывает файл output_file в режиме записи и сохраняет в него вывод команды driverquery при помощи subprocess.call.

­Затемределена функция filter_file_sistem, которая принимает один аргумент input_file. Функция открывает файл input_file в режиме чтения, читает его содержимое в список строк и разделяет первую строку на элементы. Затем происходит фильтрация списка строк, чтобы оставить только драйверы, связанные с файловой системой, и вывод этих драйверов.
­
В основном блоке кода проверяется, является ли этот скрипт главным, и создается объект парсера аументов командной строки. Затем добавляются аргументы командной строки --output-file и --input с их значениями по умолчанию и описаниями. Аргументы разбираются с помощью метода parse_args.

­Далее вызывается функция run_driverquery аргументом output_file и функция filter_file_sistem с аргументом input_file. Затем проверяется значение output_file из аргументов командной строки и выводится соответствующее сообщение (мини-тест).

В дальнейшем добавила бы еще пару тестов:

+ Проверку наличия утилиты driverquery перед ее запуском;
+ Обработчик исключений при чтении файла или выполнении команды, чтобы предотвратить возможные сбои программы


Что делает данный скрипт:

- Запускает команду driverquery /fo csv и сохраняет результаты в указанный файл;
- Определяет кодировку файла с помощью библиотеки chardet;
- Читает файл и фильтрует строки, оставляя только драйверы с типом "File System";
- Выводит отфильтрованные данные;
- Предоставляет возможность задать пути к входному и выходному файлу через аргументы командной строки.

Как работать со скриптом:

Для запуска скрипта используйте команду 

```python script.py --output=output.csv --input=input.csv, где output.csv - путь к файлу для сохранения результатов, а input.csv - путь к файлу для фильтрации.```



```# !/usr/bin/env python3
# -*- coding: <encoding name> -*-

import chardet
import argparse
import subprocess

def run_driverquery(output_file):
    # Запускает команду driverquery и выводит результаты в файл
    with open(output_file, 'w') as f:
        subprocess.call(['driverquery', '/fo', 'csv'], stdout=f)

def filter_file_system(input_file):
    with open(input_file, 'rb') as f:
        result = chardet.detect(f.read())
        encoding = result['encoding']

    with open(input_file, 'r', encoding=encoding) as f:
        lines = f.readlines()
        start = lines[0].strip().split(',')
        file_drivers = []
        for line in lines[1:]:
            driver_data = line.strip().split(',')
            if driver_data[start.index('Тип драйвера')].strip() == 'File system':
                file_drivers.append(driver_data)
            else:
                print('Type is not in list')
    return file_drivers

if __name__ == '__main__':
    parser = argparse.ArgumentParser()
    parser.add_argument('--output', dest='output_file', default='drivers.csv', help='output file path')
    parser.add_argument('--input', dest='input_file', default='drivers.csv', help='input file path')
    args = parser.parse_args()

    run_driverquery(args.output_file)
    file_system_drivers = filter_file_system(args.input_file)

    for driver_data in file_system_drivers:
        print(driver_data)

    if args.output_file == 'drivers.csv':
        print('done')
    else:
        print('error')
