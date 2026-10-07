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
