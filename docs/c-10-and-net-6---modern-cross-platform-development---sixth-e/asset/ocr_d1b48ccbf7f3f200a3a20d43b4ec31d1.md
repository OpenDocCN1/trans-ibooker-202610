```
"~/images/category@(Model.Categories[c].CategoryId).jpeg"
alt="@Model.Categories[c].CategoryName" />
<div class="carousel-caption d-none d-md-block">
<h2>@Model.Categories[c].CategoryName</h2>
<h3>@Model.Categories[c].Description</h3>
<p>
<a class="btn btn-primary"
href="/category/@Model.Categories[c].CategoryId">View</a>
</p>
</div>
</div>
}
</div>
<a class="carousel-control-prev" href="#categories"
role="button" data-slide="prev">
<span class="carousel-control-prev-icon"
aria-hidden="true"></span>
<span class="sr-only">Previous</span>
</a>
<a class="carousel-control-next" href="#categories"
role="button" data-slide="next">
<span class="carousel-control-next-icon" aria-hidden="true"></span>
<span class="sr-only">Next</span>
</a>
</div>
}
<div class="row">
<div class="col-md-12">
<h1>Northwind</h1>
<p class="lead">
We have had @Model?.VisitorCount visitors this month.
</p>
@if (Model is not null)
{
<h2>Products</h2>
<div id="product-columns">
<ul>
@foreach (Product p in @Model.Products)
{
```