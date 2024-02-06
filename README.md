# script
Напишите скрипт на Python, который  будет запускать утилиту driverquery с сохранением результатов в файл, а потом открывать этот файл и выводить только драйвера с типом File System

#!/usr/bin/env python3
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
        print('error')
