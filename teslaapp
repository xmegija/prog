using System;
using Microsoft.Data.Sqlite;

class Program
{
    public static void Main()
    {
        string connectionString = "Data Source=tesla_rental.db";

        try
        {
            var rentalCtrl = new TeslaRentalCtrl(connectionString);

            while (true)
            {
                Console.WriteLine("Choose action: 'add_car', 'register_client', 'rent_car', 'print_clients', 'print_cars', 'exit':");
                string userCommand = Console.ReadLine();

                switch (userCommand)
                {
                    case "add_car":
                        rentalCtrl.AddCar();
                        break;
                    case "register_client":
                        rentalCtrl.RegisterClient();
                        break;
                    case "rent_car":
                        rentalCtrl.RentCar();
                        break;
                    case "print_clients":
                        rentalCtrl.PrintClients();
                        break;
                    case "print_cars":
                        rentalCtrl.PrintCars();
                        break;
                    case "exit":
                        return;
                    default:
                        Console.WriteLine("Invalid command. Try again.");
                        break;
                }
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine($"An error occurred: {ex.Message}");
        }
    }
}

public class TeslaRentalCtrl
{
    private readonly string connectionString;

    public TeslaRentalCtrl(string connectionString)
    {
        this.connectionString = connectionString;
        CreateTables();
    }

    // Создание таблиц для автомобилей, клиентов и аренд
    private void CreateTables()
    {
        using (var connection = new SqliteConnection(connectionString))
        {
            connection.Open();

            var createCarsTable = connection.CreateCommand();
            createCarsTable.CommandText = @"
                CREATE TABLE IF NOT EXISTS Cars (
                    ID INTEGER PRIMARY KEY AUTOINCREMENT,
                    Model TEXT NOT NULL,
                    HourlyRate REAL NOT NULL,
                    KmRate REAL NOT NULL
                );";
            createCarsTable.ExecuteNonQuery();

            var createClientsTable = connection.CreateCommand();
            createClientsTable.CommandText = @"
                CREATE TABLE IF NOT EXISTS Clients (
                    ID INTEGER PRIMARY KEY AUTOINCREMENT,
                    FullName TEXT NOT NULL,
                    Email TEXT NOT NULL
                );";
            createClientsTable.ExecuteNonQuery();

            Console.WriteLine("Tables initialized successfully.");
        }
    }

    public void AddCar()
    {
        Console.WriteLine("Enter car model (e.g., Model 3):");
        string model = Console.ReadLine();

        Console.WriteLine("Enter hourly rate (EUR/h):");
        double hourlyRate = Convert.ToDouble(Console.ReadLine());

        Console.WriteLine("Enter kilometer rate (EUR/km):");
        double kmRate = Convert.ToDouble(Console.ReadLine());

        using (var connection = new SqliteConnection(connectionString))
        {
            connection.Open();
            var insertCmd = connection.CreateCommand();
            insertCmd.CommandText = "INSERT INTO Cars (Model, HourlyRate, KmRate) VALUES (@model, @hourlyRate, @kmRate)";
            insertCmd.Parameters.AddWithValue("@model", model);
            insertCmd.Parameters.AddWithValue("@hourlyRate", hourlyRate);
            insertCmd.Parameters.AddWithValue("@kmRate", kmRate);

            insertCmd.ExecuteNonQuery();
            Console.WriteLine("Car added successfully.");
        }
    }

    public void RegisterClient()
    {
        Console.WriteLine("Enter client's full name:");
        string fullName = Console.ReadLine();

        Console.WriteLine("Enter client's email:");
        string email = Console.ReadLine();

        using (var connection = new SqliteConnection(connectionString))
        {
            connection.Open();
            var insertCmd = connection.CreateCommand();
            insertCmd.CommandText = "INSERT INTO Clients (FullName, Email) VALUES (@fullName, @email)";
            insertCmd.Parameters.AddWithValue("@fullName", fullName);
            insertCmd.Parameters.AddWithValue("@email", email);

            insertCmd.ExecuteNonQuery();
            Console.WriteLine("Client registered successfully.");
        }
    }

    public void PrintCars()
    {
        using (var connection = new SqliteConnection(connectionString))
        {
            connection.Open();
            var selectCmd = connection.CreateCommand();
            selectCmd.CommandText = "SELECT * FROM Cars";

            using (var reader = selectCmd.ExecuteReader())
            {
                Console.WriteLine("Cars List:");
                while (reader.Read())
                {
                    Console.WriteLine($"ID: {reader["ID"]}, Model: {reader["Model"]}, Hourly Rate: {reader["HourlyRate"]}, Km Rate: {reader["KmRate"]}");
                }
            }
        }
    }

    public void PrintClients()
    {
        using (var connection = new SqliteConnection(connectionString))
        {
            connection.Open();
            var selectCmd = connection.CreateCommand();
            selectCmd.CommandText = "SELECT * FROM Clients";

            using (var reader = selectCmd.ExecuteReader())
            {
                Console.WriteLine("Clients List:");
                while (reader.Read())
                {
                    Console.WriteLine($"ID: {reader["ID"]}, Full Name: {reader["FullName"]}, Email: {reader["Email"]}");
                }
            }
        }
    }

    public void RentCar()
    {
        Console.WriteLine("Feature coming soon: Car Rental Process!");
    }
}
