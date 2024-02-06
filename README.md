# script
Напишите скрипт на Python, который  будет запускать утилиту driverquery с сохранением результатов в файл, а потом открывать этот файл и выводить только драйвера с типом File System

Этот код представляет собой скрипт Python, который выполняет две функции: сначала запускает команду driverquery с помощью subprocess и сохраняет результат в файл, а затем фильтрует содержимое этого файла и выводит по драйверам, связанным с файловой системой.
Краткое описание реализации:

Истользовала: 
- import argparse: Импортирует модуль argparse, который позволяет создавать удобные интерфейсы командной.
- import subprocess: Импортирует модуль subprocess, который позволяет выполнять внешние команды.
­

Концепция кода состоит в выполнении двух основных задач: запуск команды driverquery и сохранение ее вывода в файл, а затем фильтрация этого файла и вывод только драйверов, связанных с файловой систем.

­Реализация кода начинается с определения функции run_driverquery, которая принимает один аргумент output_file. Функция открывает файл output_file в режиме записи и сохраняет в него вывод команды driverquery при помощи subprocess.call.

­Затемределена функция filter_file_sistem, которая принимает один аргумент input_file. Функция открывает файл input_file в режиме чтения, читает его содержимое в список строк и разделяет первую строку на элементы. Затем происходит фильтрация списка строк, чтобы оставить только драйверы, связанные с файловой системой, и вывод этих драйверов.
­
В основном блоке кода проверяется, является ли этот скрипт главным, и создается объект парсера аументов командной строки. Затем добавляются аргументы командной строки --output-file и --input с их значениями по умолчанию и описаниями. Аргументы разбираются с помощью метода parse_args.

­Далее вызывается функция run_driverquery аргументом output_file и функция filter_file_sistem с аргументом input_file. Затем проверяется значение output_file из аргументов командной строки и выводится соответствующее сообщение (мини-тест).


```#!/usr/bin/env python3
# -*- coding: <encoding name> -*-
import argparse
import subprocess

def run_driverquery(output_file):
    with open(output_file, 'w') as f:
        subprocess.call(['driverquery', '/fo', 'csv'], stdout=f)

def filter_file_sistem(input_file):
    with open(input_file, 'r') as f:
        lines = f.readlines()
        start = lines[0].strip().split(',')
        file_drivers = []
        for line in lines[1:]:
            driver_data = line[0].strip().split(',')
            if driver_data[start.index('Тип драйвера')] == 'File system':
                file_drivers.append(driver_data)
            else:
                print('Type is not in list')

        for driver_data in file_drivers:
            print(driver_data)

if __name__ == 'main':
    parser = argparse.ArgumentParser()
    parser.add_argument('--output-file', dest='output_file', default='drivers.csv', help='output file path')
    parser.add_argument('--input', dest='input_file', default='drivers.csv', help='input file path')
    args = parser.parse_args()

    run_driverquery(args.output_file)
    filter_file_sistem(args.input_file)
    if args.output_file == 'drivers.csv':
        print('done')
    else:
        print('error')```
