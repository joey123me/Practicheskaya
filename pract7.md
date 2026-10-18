# Практическая работа №7. Тема: Методы. Группа П25-2.1: Трунцев Богдан.

![Result](pic12.png?raw=true "Optional title")

```using System;

class Program
{
    static void Main()
    {
        // Начальные данные
        int loadA = 500;      // нагрузка на подстанции A
        int limitB = 400;    // лимит на линии B
        double freq = 51.2;  // частота сети

        // 1. Переброска нагрузки (ref + out)
        bool overloaded;
        int losses;
        TransferLoad(ref loadA, ref limitB, 150, out overloaded, out losses);
        Console.WriteLine($"После переброски: нагрузка A={loadA}, лимит B={limitB}, потери={losses}, перегрузка={overloaded}");

        // 2. Стабилизация частоты (перегрузки)
        StabilizeFrequency(ref freq);
        Console.WriteLine($"Частота после базовой стабилизации: {freq:F1} Гц");

        double deviation;
        StabilizeFrequency(ref freq, 100, 50, out deviation);
        Console.WriteLine($"Частота с резервами: {freq:F1} Гц, отклонение={deviation:F1}");

        // 3. Рекурсивный расчёт каскада
        double cascadeRisk = CalculateCascade(5, 0.95, 0);
        Console.WriteLine($"Риск каскадного сбоя: {cascadeRisk:F2}");

        // 4. Дефицит по районам (params)
        int deficit = AnalyzeDeficit(450, 400, 480, 520, 430, 510);
        Console.WriteLine($"Суммарный дефицит (выше 450): {deficit} МВт");

        // 5. Матрица нагрузок
        int[,] grid = GenerateGrid(4, 4);
        RenderGrid(grid);
    }

    // --- 1. Переброска мощности (ref + out) ---
    static void TransferLoad(ref int load, ref int capacity, int transfer,
        out bool overloaded, out int losses)
    {
        int possible = Math.Min(load, transfer);      // нельзя перекинуть больше, чем есть
        losses = possible / 10;                      // 10% потерь
        int delivered = possible - losses;

        load -= possible;                            // уменьшаем источник
        capacity += delivered;                      // добавляем в линию
        overloaded = capacity > 500;                 // порог перегрузки
    }

    // --- 2. Перегрузки: стабилизация частоты ---
    // Вариант 1: простая подстройка к 50 Гц
    static void StabilizeFrequency(ref double freq)
    {
        if (freq > 50.0) freq -= 0.1;
        else if (freq < 50.0) freq += 0.1;
    }

    // Вариант 2: с резервами и возвратом отклонения
    static void StabilizeFrequency(ref double freq, double hydro, double thermal, out double deviation)
    {
        double boost = (hydro + thermal) * 0.001;   // условный эффект резервов
        if (freq > 50.0) freq -= boost;
        else freq += boost;

        freq = Math.Clamp(freq, 49.5, 50.5);
        deviation = Math.Abs(freq - 50.0);
    }

    // --- 3. Рекурсия: риск каскадного сбоя ---
    static double CalculateCascade(int nodes, double stress, int step)
    {
        const double failThreshold = 0.80;

        // Базовый случай: либо все узлы прошли, либо стресс уже слишком низкий
        if (step >= nodes || stress < failThreshold)
            return 1.0 - stress;

        // Рекурсивный шаг: стресс падает на 10% на каждом узле
        return CalculateCascade(nodes, stress * 0.90, step + 1);
    }

    // --- 4. params: анализ дефицита ---
    static int AnalyzeDeficit(int threshold, params int[] demands)
    {
        int total = 0;
        foreach (var d in demands)
        {
            if (d > threshold)
                total += (d - threshold);
        }
        return total;
    }

    // --- 5. Матрицы: генерация и вывод ---
    static int[,] GenerateGrid(int rows, int cols)
    {
        var grid = new int[rows, cols];
        var rnd = new Random(777);
        for (int i = 0; i < rows; i++)
            for (int j = 0; j < cols; j++)
                grid[i, j] = rnd.Next(300, 600);
        return grid;
    }

    static void RenderGrid(int[,] grid)
    {
        int rows = grid.GetLength(0);
        int cols = grid.GetLength(1);

        int maxVal = int.MinValue;
        int maxR = -1, maxC = -1;

        Console.WriteLine("\nМатрица нагрузок (МВт):");
        for (int i = 0; i < rows; i++)
        {
            Console.Write($"Строка {i}: ");
            for (int j = 0; j < cols; j++)
            {
                int v = grid[i, j];
                Console.Write($"{v,4}");
                if (v > maxVal)
                {
                    maxVal = v;
                    maxR = i;
                    maxC = j;
                }
            }
            Console.WriteLine();
        }

        Console.WriteLine($"\nМаксимальная нагрузка: {maxVal} МВт в ячейке [{maxR}, {maxC}]");
    }
}

