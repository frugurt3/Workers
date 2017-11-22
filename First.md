using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace FinalVersion
{
    using System;
    using System.Collections.Generic;
    using System.Linq;

    

        abstract class Worker
        {

            public string Name { get; set; }
            public int Id { get; set; }
            private bool StaticSalary;

            public bool staticSalary
            {
                set => StaticSalary = value;
            }

            public double Amount { get; protected set; }


            public virtual void Construct()
            {
                Console.WriteLine("Введите имя сотрудника\n");
                Name = Console.ReadLine();
                Console.WriteLine("Введите идентификационный номер сотрудника\n");
                Id = int.Parse(Console.ReadLine());

            }


        }

        class StaticWorker : Worker
        {

            public override void Construct()
            {
                Console.Clear();
                Console.WriteLine("Выбран тип - с фиксированной оплатой\n");
                base.Construct();
                Console.WriteLine("Введите ЗП сотрудника\n");
                Amount = double.Parse(Console.ReadLine());
                staticSalary = true;

            }

            public StaticWorker()
            {
                Construct();
            }
        }

        class HourWorker : Worker
        {

            public override void Construct()
            {
                Console.Clear();
                Console.WriteLine("Выбран тип - с почасовой оплатой\n");
                base.Construct();
                Console.WriteLine("Введите коэффициент для расчета ЗП сотрудника\n");
                int Coeff = int.Parse(Console.ReadLine());
                Amount = 20.8 * 8 * Coeff;
                staticSalary = false;


            }

            public HourWorker()
            {
                Construct();
            }

        }

        class Server
        {
            public static List<Worker> workers = new List<Worker>();

            public static void MassiveSorting()
            {
                Console.Clear();
                workers = Server.workers.OrderByDescending(workers => workers.Amount).ToList();
                Console.WriteLine("Сортировка завершена успешно, перевод в главное меню будет совершен после нажатия клавиши Enter");
                Console.ReadKey();
                Program.Start();
            }

            public static void AllWorkers()
            {
                Console.Clear();

                foreach (Worker worker in Server.workers)
                {
                    Console.WriteLine("Идентификатор сотрудника:" + worker.Id);
                    Console.WriteLine("Имя сотрудника:" + worker.Name);
                    Console.WriteLine("Размер ЗП (сумма):" + worker.Amount + "\n");

                }
                Console.ReadKey();
                Program.Start();
            }

            public static void DeleteWorker()

            {
                Console.Clear();

                foreach (Worker worker in Server.workers)
                {
                    Console.WriteLine("Идентификатор сотрудника:" + worker.Id);
                    Console.WriteLine("Имя сотрудника:" + worker.Name);
                    Console.WriteLine("Размер ЗП (сумма):" + worker.Amount + "\n");
                }

                Console.WriteLine("\nВведите имя сотрудника, которого хотите удалить (с учетом регистра)");
                var nameToDelete = Console.ReadLine();

                var delete = workers.FirstOrDefault(name => name.Name == nameToDelete);

                if (delete != null)
                {
                    workers.Remove(delete);
                    Console.WriteLine("Элемент успешно удален, для возврата в главное меню нажмите Enter.");
                }
                else
                {
                    Console.WriteLine("Не найдено совпадающих элементов. Возврат в главное меню. Нажмите Enter.");
                }

                Console.ReadKey();
                Program.Start();
            }

            public static void FirstFiveNames()
            {

                Console.Clear();
                IEnumerable<Worker> firstfive = workers.Take(5);
                foreach (var i in firstfive)
                    Console.WriteLine(i.Name);

                Console.WriteLine("\nОперация выполнена, нажмите Enter для перехода в главное меню программы");
                Console.ReadKey();

                Program.Start();

            }

            public static void LastThreeID()
            {
                Console.Clear();

                var lastThreeId = Enumerable.Reverse(workers).Take(3).Reverse().ToList();

                foreach (var i in lastThreeId)
                    Console.WriteLine(i.Id);

                Console.WriteLine("\nОперация выполнена, нажмите Enter для перехода в главное меню программы");
                Console.ReadKey();
                Program.Start();
            }
        }
        class Program
        {

            Server workers = new Server();
            static void Main(string[] args)
            {
                Start();
            }

            public static void Start()
            {
                
                Console.Clear();
                Console.WriteLine("В вашем списке работников в данный момент " + Server.workers.Count + " элементов.\nДля совершения действия выберите нужный вариант:" +
                    "\n1: Добавить работника с фиксированной ставкой" +
                    "\n2: Добавить работника с почасовой ставкой" +
                    "\n3: Упорядочить список сотрудников" +
                    "\n4: Удалить сотрудника" +
                    "\n5: Вывести последние 3 идентификатора работников из полученного после упорядочивания списка" +
                    "\n6: Вывести весь список сотрудников" +
                    "\n7: Вывести первые 5 имен списка" +
                    "\n8: Выход из программы");

                string mainValue = Console.ReadLine();
                switch (mainValue)
                {
                    case "1":
                        var newStaticWorker = new StaticWorker();
                        Server.workers.Add(newStaticWorker);
                        Start();
                        break;

                    case "2":
                        var newHourWorker = new HourWorker();
                        Server.workers.Add(newHourWorker);
                        Start();
                        break;

                    case "3":
                        Server.MassiveSorting();
                        break;

                    case "4":
                        Server.DeleteWorker();
                        break;

                    case "5":
                        Server.LastThreeID();
                        break;

                    case "6":
                        Server.AllWorkers();
                        break;

                    case "7":
                        Server.FirstFiveNames();
                        break;

                    case "8":
                        Environment.Exit(8);
                        break;

                    default:
                        Console.Clear();
                        Console.WriteLine("Некорректный выбор пункта меню, возврат к главному меню после нажания клавиши Enter");
                        Console.ReadKey();
                        Start();
                        break;
                }



            }

        }
    }



