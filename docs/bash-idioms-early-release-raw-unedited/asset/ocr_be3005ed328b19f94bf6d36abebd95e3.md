```
echo "Creating & reading random word list in: $WORD_FILE"

# Create a list of words to use for the hash example
mylist=(foo bar baz one two three four)

# Loop, and randomly pick elements out of the lips
range="${#mylist[@]}"
for ((i=0; i<35; i++)); do
  random_element="$(( $RANDOM % $range ))"
  echo "${mylist[$random_element]}" >> $WORD_FILE
done

# Read the word list into a hash
while read line; do
  (( myhash[$line]++ ))
done < $WORD_FILE

echo -e "\nUnique words from: $WORD_FILE"
for key in "${!myhash[@]}"; do
  echo "$key"
done | sort

echo -e "\nWord counts, ordered by word, from: $WORD_FILE"
for key in "${!myhash[@]}"; do
  printf "%s\t%d\n" $key ${myhash[$key]}
done | sort

echo -e "\nWord counts, ordered by count, from: $WORD_FILE"
for key in "${!myhash[@]}"; do
  printf "%s\t%d\n" $key ${myhash[$key]}
done | sort -k2,2n
```