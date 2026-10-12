# Practicheskaya 2
```using System;

class PyramidAdventure
{
    static void Main()
    {
        string[] inventory = { "Факел", "Антидот", "Древний амулет", "Кирка", "Масляная лампа" };

        Console.WriteLine("Снаряжение археолога:");
        foreach (string item in inventory)
        {
            Console.WriteLine("- " + item);
        }
        Console.WriteLine();

        string[] guards = { "Ядовитые скорпионы", "Охранная мумия", "Проклятый жрец" };
        int[] healths = { 25, 65, 125 };
        int[] cursePower = { 10, 20, 40 };

        Console.WriteLine("Опасности гробницы:");
        for (int i = 0; i < guards.Length; i++)
        {
            Console.WriteLine($"{guards[i]} | HP: {healths[i]} | Сила проклятия: {cursePower[i]}");
        }
        Console.WriteLine();

        int[] artifactWeights = { 3, 7, 5, 9, 4 }; // веса в условных единицах

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

        int[] roomSteps = { 12, 8, 15, 10 };

        int totalSteps = 0;
        for (int i = 0; i < roomSteps.Length; i++)
        {
            totalSteps += roomSteps[i];
        }
        double averageSteps = (double)totalSteps / roomSteps.Length;
        Console.WriteLine($"Среднее количество шагов на зал: {averageSteps:F2}");
        Console.WriteLine();

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

        Array.Sort(artifactWeights);
        Console.WriteLine("Веса артефактов после сортировки (по возрастанию):");
        foreach (int w in artifactWeights)
        {
            Console.Write(w + " ");
        }
        Console.WriteLine("\n");

        Array.Reverse(roomSteps);
        Console.WriteLine("Обратный маршрут (шаги в обратном порядке):");
        foreach (int s in roomSteps)
        {
            Console.Write(s + " ");
        }
        Console.WriteLine();
    }
}

