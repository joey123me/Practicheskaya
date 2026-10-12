# Practicheskaya


```using System;

class Program
{
    static void Main()
    {
        int playerHp = 100;
        int maxHp = 100;
        int mana = 20;
        int maxMana = 20;
        int restoreCount = 2;
        bool isDefending = false;

        Random rnd = new Random();

        Console.WriteLine("=== Простая битва ===");

        // Волны врагов (for): 3 волны, сила врага растёт
        for (int wave = 1; wave <= 3; wave++)
        {
            int enemyHp = 60 + (wave - 1) * 15;
            int minDmg = 8 + (wave - 1) * 2;
            int maxDmg = 14 + (wave - 1) * 3;

            Console.WriteLine($"\n--- ВОЛНА {wave} --- Враг: {enemyHp} HP");

            // Главный цикл боя (while): пока живы оба
            while (playerHp > 0 && enemyHp > 0)
            {
                // Визуализация шкал (for): по 10 символов
                Console.Write("Здоровье: [");
                for (int i = 0; i < 10; i++)
                {
                    if (i < playerHp / 10)
                        Console.Write("#");
                    else
                        Console.Write("-");
                }
                Console.WriteLine($"] ({playerHp}/{maxHp})");

                Console.Write("Мана:     [");
                for (int i = 0; i < 10; i++)
                {
                    if (i < mana / 2)
                        Console.Write("#");
                    else
                        Console.Write("-");
                }
                Console.WriteLine($"] ({mana}/{maxMana})");

                // Меню и валидация (do-while)
                int action;
                bool isValid;
                do
                {
                    Console.WriteLine("\nВыберите действие:");
                    Console.WriteLine("1 — Обычная атака");
                    Console.WriteLine("2 — Спецспособность (10 маны)");
                    Console.WriteLine("3 — Защита (в 2 раза меньше урона)");
                    Console.WriteLine($"4 — Восстановить ману (осталось: {restoreCount})");
                    Console.Write("Ваш выбор: ");

                    isValid = int.TryParse(Console.ReadLine(), out action) && action >= 1 && action <= 4;

                    if (!isValid)
                    {
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.WriteLine("Ошибка: введите 1–4!\n");
                        Console.ResetColor();
                    }
                } while (!isValid);

                // Логика действий (switch)
                isDefending = false;

                switch (action)
                {
                    case 1: // Обычная атака
                        int dmg = rnd.Next(10, 21);
                        enemyHp -= dmg;
                        Console.WriteLine($"Вы ударили на {dmg} урона. Враг: {Math.Max(0, enemyHp)} HP");
                        break;

                    case 2: // Спецспособность
                        if (mana >= 10)
                        {
                            mana -= 10;
                            int specialDmg = rnd.Next(20, 31);
                            enemyHp -= specialDmg;
                            Console.WriteLine($"Спецспособность! {specialDmg} урона. Мана: {mana}/{maxMana}");
                        }
                        else
                        {
                            Console.ForegroundColor = ConsoleColor.Yellow;
                            Console.WriteLine("Не хватает маны!");
                            Console.ResetColor();
                        }
                        break;

                    case 3: // Защита
                        isDefending = true;
                        Console.WriteLine("Вы встали в защиту. Получаемый урон уменьшится вдвое.");
                        break;

                    case 4: // Восстановление маны
                        if (restoreCount > 0)
                        {
                            restoreCount--;
                            mana = Math.Min(maxMana, mana + 10);
                            Console.WriteLine($"Мана восстановлена! Осталось восстановлений: {restoreCount}. Мана: {mana}/{maxMana}");
                        }
                        else
                        {
                            Console.ForegroundColor = ConsoleColor.Yellow;
                            Console.WriteLine("Восстановления закончились!");
                            Console.ResetColor();
                        }
                        break;
                }

                // Ход врага (если жив)
                if (enemyHp > 0)
                {
                    int enemyDmg = rnd.Next(minDmg, maxDmg + 1);
                    if (isDefending)
                    {
                        enemyDmg /= 2;
                        Console.WriteLine($"В защите вы получили {enemyDmg} урона.");
                    }
                    else
                    {
                        Console.WriteLine($"Враг ударил вас на {enemyDmg} урона.");
                    }

                    playerHp -= enemyDmg;
                    Console.WriteLine($"Ваше здоровье: {Math.Max(0, playerHp)} HP\n");
                }
            }

            // Проверка после волны
            if (playerHp <= 0)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("You are dead. Game Over.");
                Console.ResetColor();
                return;
            }
            else
            {
                Console.ForegroundColor = ConsoleColor.Green;
                Console.WriteLine($"Волна {wave} пройдена!");
                Console.ResetColor();
            }
        }

        Console.ForegroundColor = ConsoleColor.Cyan;
        Console.WriteLine("Победа! Все волны пройдены.");
        Console.ResetColor();
    }
}
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

