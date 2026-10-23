```
```c
printf("\nShowing the most important differences:\n");

for(a = 0; a < 16; a++)
{
    for(b = 0; b < 16; b++)
    {
        if (characters[a][b] == 6)
        {
            printf("  6/16:  %i {--} %i\n", a, b);
        }
    }
}

void generate_characters_data(int input_differences,
                              int output_differences)
{
    printf("\nIntermediate values generated using the\ndifferentials(%i --> %i):\n", input_differences,
           output_differences);

    characters_data_max = 0;
    int f;
    for(f = 0; f < 16; f++)
    {
        int computations = f ^ input_differences;

        if ((theSBox[f] ^ theSBox[computations]) ==
            output_differences)
        {
            printf("  The possibility:  %i + %i {--}\n"
                   "                     %i + %i\n", f, computations,
                   theSBox[f], theSBox[computations]);
            characters_dataSet_o[characters_data_max] = f;
            characters_data_max++;
        }
    }
}
```
```