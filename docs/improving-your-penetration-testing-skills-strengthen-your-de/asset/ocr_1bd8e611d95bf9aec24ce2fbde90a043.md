```
store_wmic_loot(result, cmd)
end
end

def store_wmic_loot(result_text, cmd)
    command_log = store_loot("host.command.wmic",
                            "text/plain",
                            session,
                            result_text,
                            "#{cmd.gsub(/\. |\| \\s/,"_") }.txt",
                            "Command Output \\ 'wmic #{cmd.chomp}\\'")
    print_status("Command output saved to: #{command_log}")
end
end
```

Now that we have built some background about module building, our next step will be to analyze the module. It is highly recommended that you look at existing modules if you have to learn and dive deeper into module and platform development.

# How it works...

Let's start with the analysis of the main script body to understand how it works:

+   1. The module starts by verifying if a resource file is supplied. If so, it will run a WMIC query for each line and store the results:

```
if datastore['RESOURCE']
  if ::File.exist?(datastore['RESOURCE'])
    ::File.open(datastore['RESOURCE']).each_line
    do |cmd|
      next if cmd.strip.length < 1
      next if cmd[0,1] == "#"
      print_status "Running command #{cmd.chomp}"
      result = wmic_query(cmd.chomp)
      store_wmic_loot(result, cmd)
    end
  else
    raise "Resource File does not exists!"
  end
```