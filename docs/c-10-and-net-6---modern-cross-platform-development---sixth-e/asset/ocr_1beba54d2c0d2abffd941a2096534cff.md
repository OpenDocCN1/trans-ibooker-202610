```
<td>
    @Html.DisplayFor(modelItem => p.ProductName)
</td>
<td>
    @Html.DisplayFor(modelItem => p.UnitPrice)
</td>
<td>
    @Html.DisplayFor(modelItem => p.UnitsInStock)
</td>
</tr>
}
<tbody>
</table>
}
```

5. In the Views/Home folder, open Index.cshtml.

6. Add the following form element below the visitor count and above the Products heading and its listing of products. This will provide a form for the user to enter a price. The user can then click Submit to call the action method that shows only products that cost more than the entered price:

```
<h3>Query products by price</h3>
<form asp-action="ProductsThatCostMoreThan" method="GET">
    <input name="price" placeholder="Enter a product price" />
    <input type="submit" />
</form>
```

7. Start the website.

8. On the home page, enter a price in the form, for example, 50, and then click on Submit.

9. Note the table of the products that cost more than the price that you entered, as shown in Figure 15.9:

![](bbox=[0.17, 0.67, 0.93, 0.89])

Figure 15.9: A filtered list of products that cost more than £50

10. Close Chrome and shut down the web server.