# Command-line Parser
A simple command-line parser for .NET. Supports both long and short named options, including flags.

## Samples
### Parsing command-line arguments:
```csharp
private static void ParseArguments(string[] args)
{
    if (CLI.Parse(args) == 1)
    {
        Console.WriteLine("Error while parsing command-line arguments.");
        return;
    }

    foreach (var option in CLI.Options)
    {
        switch (option.Key)
        {
            case "--debug":
                Trace.Listeners.Add(new ConsoleTraceListener());
                Debug.WriteLine("Debugging...");
                break;
            case "-c" or "--config":
                IniPath = option.Value;
                break;
            case "-h" or "--help":
                Debug.WriteLine("Options: --debug, -c|--config, -h|--help.\n");
                break;
        }
    }

    Console.WriteLine("Parsing CLI complete!\n");
}
```

### Parsing command-line arguments using events:
```csharp
private static void ParseArguments(string[] args)
{
    CLI.OnParse += CLI_OnParse;
    CLI.OnParseError += (msg) => Console.WriteLine(msg);

    if (CLI.Parse(args) != 1)
        Console.WriteLine("Parsing CLI complete!\n");
}

private static void CLI_OnParse(string option, string value)
{
    switch (option)
    {
        case "--debug":
            Trace.Listeners.Add(new ConsoleTraceListener());
            Debug.WriteLine("Debugging...");
            break;
        case "-c" or "--config":
            IniPath = value;
            break;
        case "-h" or "--help":
            Debug.WriteLine("Options: --debug, -c|--config, -h|--help.\n");
            break;
    }
}
```
