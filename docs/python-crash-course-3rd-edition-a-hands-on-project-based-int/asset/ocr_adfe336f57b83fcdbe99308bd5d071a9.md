Note that the order of the parameters in the function definition had to be changed. Because the default value makes it unnecessary to specify a type of animal as an argument, the only argument left in the function call is the pet’s name. Python still interprets this as a positional argument, so if the function is called with just a pet’s name, that argument will match up with the first parameter listed in the function’s definition. This is the reason the first parameter needs to be `pet_name`.

The simplest way to use this function now is to provide just a dog’s name in the function call:

```
describe_pet('willie')
```

This function call would have the same output as the previous example. The only argument provided is 'willie', so it is matched up with the first parameter in the definition, `pet_name`. Because no argument is provided for `animal_type`, Python uses the default value 'dog'.

To describe an animal other than a dog, you could use a function call like this:

```
describe_pet(pet_name='harry', animal_type='hamster')
```

Because an explicit argument for `animal_type` is provided, Python will ignore the parameter’s default value.

> **NOTE**
> 
> *When you use default values, any parameter with a default value needs to be listed after all the parameters that don’t have default values. This allows Python to continue interpreting positional arguments correctly.*