''' 

using System;

namespace VariableScope
{
    class Program
    {
        private static string helloClass = "Hello, class!";

        static void Main(string[] args)
        {
            string helloLocal = "Hello, local!";
            Console.WriteLine(helloLocal); // Hello local
            Console.WriteLine(Program.helloClass); // Hello Class
            DoStuff();
        }

        static void DoStuff()
        {
            Console.WriteLine("A message from DoStuff: " + Program.helloClass); // Hello Class
        }
    }
}
'''
