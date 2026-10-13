# Практическая работа №6. Тема: Двумерные массивы. Группа П25-2.1: Трунцев Богдан, Дмитрий Карпухин.

![Result](pic12.png?raw=true "Optional title")

```using System;

class Program
{
    static void Main()
    {
        Console.ForegroundColor = ConsoleColor.DarkMagenta;
        Console.WriteLine("АРКТИЧЕСКАЯ МЕТЕОСТАНЦИЯ");
        Console.WriteLine("   СНЕГОВОЙ ПОКРОВ  ");
        Console.ResetColor();
        Console.WriteLine("Символика: . — наст, * — снежный перемет, D — сугроб выше метра, H — домик метеорологов");
        Console.WriteLine();

        // Создание матрицы высоты сугробов
        int[,] snowDepth = {
            { 50, 80, 120, 60, 90 },
            { 70, 110, 40, 130, 60 },
            { 100, 90, 140, 80, 50 },
            { 30, 120, 70, 60, 110 },
            { 80, 50, 90, 130, 40 }
        };

        int rows = snowDepth.GetLength(0);
        int cols = snowDepth.GetLength(1);

        // Создание матрицы ландшафта
        char[,] landscape = new char[rows, cols];

        // Заполнение ландшафта на основе глубины
        for (int i = 0; i < landscape.GetLength(0); i++)
        {
            for (int j = 0; j < landscape.GetLength(1); j++)
            {
                if (snowDepth[i, j] > 100)
                    landscape[i, j] = 'D';
                else
                    landscape[i, j] = '.';
            }
        }

        // Размещение метеостанции

        landscape[2, 2] = 'H';

        // Вывод обеих матриц
        Console.WriteLine();
        Console.WriteLine("ГЛУБИНА СНЕГА (см):");

        for (int i = 0; i < snowDepth.GetLength(0); i++)
        {
            for (int j = 0; j < snowDepth.GetLength(1); j++)
                Console.Write($"{snowDepth[i, j],4}");

            Console.WriteLine();
        }

        Console.WriteLine();
        Console.WriteLine("ЛАНДШАФТ:");

        for (int i = 0; i < landscape.GetLength(0); i++)
        {
            for (int j = 0; j < landscape.GetLength(1); j++)
            {
                if (landscape[i, j] == 'H')
                    Console.ForegroundColor = ConsoleColor.Yellow;
                else if (landscape[i, j] == 'D')
                    Console.ForegroundColor = ConsoleColor.Cyan;
                else
                    Console.ForegroundColor = ConsoleColor.Gray;

                Console.Write($"{landscape[i, j],4}");
                Console.ResetColor();
            }

            Console.WriteLine();
        }

        // Расчет средней толщины снежного покрова
        int sum = 0;

        for (int i = 0; i < snowDepth.GetLength(0); i++)
        {
            for (int j = 0; j < snowDepth.GetLength(1); j++)
                sum += snowDepth[i, j];
        }

        double average = (double)sum / (rows * cols);
        Console.WriteLine();
        Console.WriteLine($"Средняя толщина снега: {average:F2} см");

        // Поиск координаты рейки с рекордным уровнем сугроба
        int max = snowDepth[0, 0];
        int maxX = 0;
        int maxY = 0;

        for (int i = 0; i < snowDepth.GetLength(0); i++)
        {
            for (int j = 0; j < snowDepth.GetLength(1); j++)
            {
                if (snowDepth[i, j] > max)
                {
                    max = snowDepth[i, j];
                    maxX = i;
                    maxY = j;
                }
            }
        }

        Console.WriteLine($"Максимальная глубина снега: {max} см");
        Console.WriteLine($"Координаты рекордной рейки: [{maxX}, {maxY}]");

        // Подсчет сугробов-гигантов D на внешнем периметре поля
        int count = 0;

        for (int i = 0; i < landscape.GetLength(0); i++)
        {
            for (int j = 0; j < landscape.GetLength(1); j++)
            {
                if ((i == 0 || i == rows - 1 || j == 0 || j == cols - 1)
                    && landscape[i, j] == 'D')
                    count++;
            }
        }

        Console.WriteLine($"Сугробов D на внешнем периметре: {count}");

        // Работа снегоуборщика
        Console.WriteLine();
        Console.WriteLine("РАБОТА СНЕГОУБОРЩИКА");
        Console.WriteLine("Введите номер строки от 0 до 4:");

        int x = Convert.ToInt32(Console.ReadLine());

        Console.WriteLine("Введите номер столбца от 0 до 4:");
        int y = Convert.ToInt32(Console.ReadLine());

        if (x >= 0 && x < rows && y >= 0 && y < cols)
        {
            snowDepth[x, y] = 0;
            Console.WriteLine("Снег в выбранной точке убран.");
        }
        else
        {
            Console.WriteLine("Ошибка: координаты находятся за пределами поля.");
        }

        // Очистка побочной диагонали
        for (int i = 0; i < snowDepth.GetLength(0); i++)
            snowDepth[i, cols - 1 - i] = 0;

        Console.WriteLine();
        Console.WriteLine("ГЛУБИНА СНЕГА ПОСЛЕ ОЧИСТКИ ДИАГОНАЛИ:");

        for (int i = 0; i < snowDepth.GetLength(0); i++)
        {
            for (int j = 0; j < snowDepth.GetLength(1); j++)
                Console.Write($"{snowDepth[i, j],4}");

            Console.WriteLine();
        }
    }
}
