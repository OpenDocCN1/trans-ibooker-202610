```
###mylist=(bar)                # Would overwrite mylist[0]
mylist+=(bar)                  # mylist[1]
mylist+=(baz)                  # mylist[2]
mylist+=(one two)              # mylist[3] AND mylist[4]
mylist+=("three four")       # mylist[5]

# OR
# mylist=(bar baz one two "three four")  # Note "foo" is
already there

# Display or dump the values
③
echo -e "\nThe element count is: ${#mylist[@]} or ${#mylist[*]}"

echo -e "\nThe length of element [5] is: ${#mylist[5]}"

echo -e "\nDump or list:"
declare -p mylist
echo -n      "${mylist[@]}	 = " ; printf "%q|" ${mylist[@]}
echo -en  "\n\${mylist[*]}	 = " ; printf "%q|" ${mylist[*]}
echo -en "\n\"${mylist[@]}\" = " ; printf "%q|" "${mylist[@]}"
echo -en "\n\"${mylist[*]}\" = " ; printf "%q|" "${mylist[*]}"
echo -e "\t# But this is broken!"  # Previous line is bad and no newline
# See `help printf` or chapter 6, we need this to show the correct words:
# %q     quote the argument in a way that can be reused as shell input

# Iterate over the values
④
echo -e "\nforeach \"\${!mylist[@]}\":"
for element in "${!mylist[@]}"; do
    echo -e "\tElement: $element; value: ${mylist[$element]}"
done

echo -e "\nBut don't do this: \${mylist[*]}"
for element in ${mylist[*]}; do
    echo -e "\tElement: $element; value: ${mylist[$element]}"
done
```