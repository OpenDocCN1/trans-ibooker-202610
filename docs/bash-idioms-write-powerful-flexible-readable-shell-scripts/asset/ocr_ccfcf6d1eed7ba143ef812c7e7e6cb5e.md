```
#- --------------------------------------------------
    ;; 
    : ) echo "error: no argument supplied" ;;
    * ) 
    echo "error: unknown option $OPTARG"
    echo "      valid options are: aov"
    ;;
esac
done
shift $((OPTIND -1))
```

How does this support long-format options? What’s really going on here?

+   1. Consider that getopts was designed to handle single-character options (e.g., -a). Adding the minus sign to the option list means that -- will be recognized as a valid option.

+   2. Any remaining characters after the two minus signs are considered the “argument” to the -- option and are put in the $OPTARG variable. We can use our inner case statement to match the value of $OPTARG to the long option names that we want to implement.

+   3. What happens, then, for the options, like --outfile, that need a user-supplied argument? If the argument is supplied with the equals sign syntax (e.g., --outfile=my.txt), then getopts assigns that entire string (after the --) to OPTARG. We can extract the argument from the option string using bash’s parameter expansion string manipulation (see “Parameter Expansion” on page 32), removing (\#) all of the characters (\*) up to, and including, the equals sign (=) via the expression that uses all of these symbols. That leaves us with just the characters after the equals sign, which would be the argument that we’re after. We might also have coded it to be more literal with the string we want to remove by coding it as OFILE="${OPTARG#outfile=}" to remove that text from the front of OPTARG.

+   4. When the argument is supplied as a separate word, the second outfile pattern in our case statement will be taken. Here we make use of a variable, $OPTIND, that getopts uses to keep track of its parsing. The filename argument is retrieved indirectly using ${!OPTIND}. How does that work?

+   5. When using getopts, the shell variable $OPTIND holds the index of the next argument to be processed by getopts. The ! says to use indirection—take the value of $OPTIND and use that as the name of the variable to retrieve. For example, if --output was the third argument encountered by getopts, then, at that point, $OPTIND would have the value 4, and, therefore, ${!OPTIND} would return the value of ${4}, which would be the next argument, the filename.