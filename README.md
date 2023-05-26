using System;
using System.Collections.Generic;

namespace CommunitySports
{
    class Program
    {
        static List<Event> events = new List<Event>();

        static void Main(string[] args)
        {
            // Welcome message
            Console.WriteLine("Welcome to the Community Sports Organizer!");

            while (true)
            {
                // Display options menu
                Console.WriteLine("\nPlease select an option from the menu below:");
                Console.WriteLine("1. Create a new event");
                Console.WriteLine("2. View upcoming events");
                Console.WriteLine("3. View past events");
                Console.WriteLine("4. Join an event");
                Console.WriteLine("5. Leave an event");
                Console.WriteLine("6. Provide feedback");
                Console.WriteLine("7. Exit");

                // Get user input
                int option = int.Parse(Console.ReadLine());

                // Handle user input
                switch (option)
                {
                    case 1:
                        CreateEvent();
                        break;
                    case 2:
                        ViewUpcomingEvents();
                        break;
                    case 3:
                        ViewPastEvents();
                        break;
                    case 4:
                        JoinEvent();
                        break;
                    case 5:
                        LeaveEvent();
                        break;
                    case 6:
                        ProvideFeedback();
                        break;
                    case 7:
                        // Exit the program
                        Console.WriteLine("Goodbye!");
                        return;
                    default:
                        Console.WriteLine("Invalid option, please try again.");
                        break;
                }
            }
        }

        static void CreateEvent()
        {
            // Get event details from the user
            Console.Write("Enter event name: ");
            string name = Console.ReadLine();
            Console.Write("Enter event location: ");
            string location = Console.ReadLine();
            Console.Write("Enter event date (MM-DD-YYYY): ");
            DateTime date = DateTime.Parse(Console.ReadLine());

            // Create the event and add it to the list
            Event newEvent = new Event(name, location, date);
            events.Add(newEvent);

            Console.WriteLine("Event created successfully.");
        }

        static void ViewUpcomingEvents()
        {
            Console.WriteLine("Upcoming events:");

            foreach (Event ev in events)
            {
                if (ev.Date >= DateTime.Today)
                {
                    Console.WriteLine($"{ev.Name} ({ev.Date.ToString("D")}) - {ev.Location}");
                    Console.WriteLine($"Attendees: {string.Join(", ", ev.Attendees)}");
                }
            }
        }

        static void ViewPastEvents()
        {
            Console.WriteLine("Past events:");

            foreach (Event ev in events)
            {
                if (ev.Date < DateTime.Today)
                {
                    Console.WriteLine($"{ev.Name} ({ev.Date.ToString("D")}) - {ev.Location}");
                    Console.WriteLine($"Attendees: {string.Join(", ", ev.Attendees)}");
                }
            }
        }

        static void JoinEvent()
        {
            Console.WriteLine("Enter the name of the event you want to join:");
            string eventName = Console.ReadLine();

            // Find the event with the specified name
            Event ev = events.Find(e => e.Name == eventName);

            if (ev == null)
            {
                Console.WriteLine($"Event '{eventName}' not found.");
                return;
            }

            Console.WriteLine("Enter your name:");
            string name = Console.ReadLine();
            ev.Attendees.Add(name);
            Console.WriteLine($"{name} has successfully joined the event '{eventName}'.");
        }

        static void LeaveEvent()
        {
            Console.WriteLine("Enter the name of the event you want to leave:");
            string eventName = Console.ReadLine();

            // Find the event with the specified name
            Event ev = events.Find(e => e.Name == eventName);

            if (ev == null)
            {
                Console.WriteLine($"Event '{eventName}' not found.");
                return;
            }

            Console.WriteLine("Enter your name:");
            string name = Console.ReadLine();
            bool result = ev.Attendees.Remove(name);
            if (result)
            {
                Console.WriteLine($"{name} has successfully left the event '{eventName}'.");
            }
            else
            {
                Console.WriteLine($"{name} was not found in the attendees list for event '{eventName}'.");
            }
        }

        static void ProvideFeedback()
        {
            Console.WriteLine("Enter the name of the event you want to provide feedback for:");
            string eventName = Console.ReadLine();

            // Find the event with the specified name
            Event ev = events.Find(e => e.Name == eventName);

            if (ev == null)
            {
                Console.WriteLine($"Event '{eventName}' not found.");
                return;
            }

            Console.WriteLine("Enter your feedback:");
            string feedback = Console.ReadLine();
            ev.Feedback.Add(feedback);

            Console.WriteLine($"Thank you for your feedback on the event '{eventName}'.");
        }
    }

    class Event
    {
        public string Name { get; set; }
        public string Location { get; set; }
        public DateTime Date { get; set; }
        public List<string> Attendees { get; set; }
        public List<string> Feedback { get; set; }

        public Event(string name, string location, DateTime date)
        {
            Name = name;
            Location = location;
            Date = date;
            Attendees = new List<string>();
            Feedback = new List<string>();
        }
    }
}
