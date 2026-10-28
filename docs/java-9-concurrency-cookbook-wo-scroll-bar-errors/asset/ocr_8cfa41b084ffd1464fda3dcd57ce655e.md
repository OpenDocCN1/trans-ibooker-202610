14. If the difference between the last and first attributes is greater than or equal to 10, create two new Task objects, one to process the first half of the products and the other to process the second half, and execute them in ForkJoinPool using the invokeAll() method:

```
} else {
    int middle=(last+first)/2;
    System.out.printf("Task: Pending tasks:%s\n",
                    getQueuedTaskCount());
    Task t1=new Task(products, first,middle+1, increment);
    Task t2=new Task(products, middle+1,last, increment);
    invokeAll(t1, t2);
}
```

15. Implement the updatePrices() method. This method updates the products that occupy the positions between the values of the first and last attributes in the list of products:

```
private void updatePrices() {
    for (int i=first; i<last; i++){
        Product product=products.get(i);
        product.setPrice(product.getPrice()*(1+increment));
    }
}
```

16. Implement the main class of the example by creating a class named Main and add the main() method to it:

```
public class Main {
    public static void main(String[] args) {
```

17. Create a list of 10000 products using the ProductListGenerator class:

```
ProductListGenerator generator=new ProductListGenerator();
List<Product> products=generator.generate(10000);
```

18. Create a new Task object to update the prices of all the products in the list. The parameter first takes the value 0 and the last parameter takes the value 10000 (the size of the product list):

```
Task task=new Task(products, 0, products.size(), 0.20);
```

19. Create a ForkJoinPool object using the constructor without parameters:

```
ForkJoinPool pool=new ForkJoinPool();
```