# Practicheskaya 2
```using System;

class PyramidAdventure
{
    static void Main()
    {
        // 1. Строковый массив снаряжения археолога из 5 предметов
        string[] inventory = { "Факел", "Антидот", "Древний амулет", "Кирка", "Масляная лампа" };

        // Вывод инвентаря через foreach
        Console.WriteLine("Снаряжение археолога:");
        foreach (string item in inventory)
        {
            Console.WriteLine("- " + item);
        }
        Console.WriteLine();

        // 2. Синхронные массивы для опасностей гробницы: страж, стойкость (HP), сила проклятия
        string[] guards = { "Ядовитые скорпионы", "Охранная мумия", "Проклятый жрец" };
        int[] healths = { 25, 65, 125 };
        int[] cursePower = { 10, 20, 40 };

        Console.WriteLine("Опасности гробницы:");
        for (int i = 0; i < guards.Length; i++)
        {
            Console.WriteLine($"{guards[i]} | HP: {healths[i]} | Сила проклятия: {cursePower[i]}");
        }
        Console.WriteLine();

        // 3. Массив веса найденных золотых артефактов (5 находок)
        int[] artifactWeights = { 3, 7, 5, 9, 4 }; // веса в условных единицах

        // 4. Проверка наличия "Антидот" через Array.IndexOf при укусе скорпиона
        int indexOfAntidote = Array.IndexOf(inventory, "Антидот");
        if (indexOfAntidote != -1)
        {
            Console.WriteLine("Антидот найден — можно нейтрализовать яд!");
        }
        else
        {
            Console.WriteLine("Антидота нет — здоровье стремительно падает...");
        }
        Console.WriteLine();

        // 5. Шаги до выхода из залов гробницы (4 перехода)
        int[] roomSteps = { 12, 8, 15, 10 };

        // Среднее количество шагов на преодоление одного зала
        int totalSteps = 0;
        for (int i = 0; i < roomSteps.Length; i++)
        {
            totalSteps += roomSteps[i];
        }
        double averageSteps = (double)totalSteps / roomSteps.Length;
        Console.WriteLine($"Среднее количество шагов на зал: {averageSteps:F2}");
        Console.WriteLine();

        // 6. Поиск самого тяжёлого артефакта стандартным алгоритмом (без готовых методов)
        int maxWeight = artifactWeights[0];
        for (int i = 1; i < artifactWeights.Length; i++)
        {
            if (artifactWeights[i] > maxWeight)
            {
                maxWeight = artifactWeights[i];
            }
        }
        Console.WriteLine($"Самый тяжёлый артефакт весит: {maxWeight}");
        Console.WriteLine();

        // 7. Сортировка массива весов артефактов по возрастанию
        Array.Sort(artifactWeights);
        Console.WriteLine("Веса артефактов после сортировки (по возрастанию):");
        foreach (int w in artifactWeights)
        {
            Console.Write(w + " ");
        }
        Console.WriteLine("\n");

        // 8. Разворот массива шагов для обратного маршрута
        Array.Reverse(roomSteps);
        Console.WriteLine("Обратный маршрут (шаги в обратном порядке):");
        foreach (int s in roomSteps)
        {
            Console.Write(s + " ");
        }
        Console.WriteLine();
    }
}

