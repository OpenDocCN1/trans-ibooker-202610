categoryid), then initiate an AJAX GET request. We pass the required information over to the URL `/rango/search_add_page/`.

Given the appropriate URL mapping (that we will leave for you to implement), we then pass control over to the new view. In this sample solution, we call the view `SearchAddPageView`. This implements the one required `get()` method.

```
class SearchAddPageView(View):
    @method_decorator(login_required)
    def get(self, request):
        category_id = request.GET['category_id']
        title = request.GET['title']
        url = request.GET['url']

        try:
            category = Category.objects.get(id=int(category_id))
        except Category.DoesNotExist:
            return HttpResponse('Error - category not found.')
        except ValueError:
            return HttpResponse('Error - bad category ID.')

        p = Page.objects.get_or_create(category=category,
                                       title=title,
                                       url=url)

        pages = Page.objects.filter(category=category).order_by('-views')
        return render(request, 'rango/page_listing.html', {'pages': pages})
```

This sample class-based view complies with all requirements – it only allows those who are logged into to add a page, and it expects input from the client with key names as per the JQuery AJAX call we implemented previously. While error handling is very rudimentary (look at the `except` blocks), it does at least catch basic error cases. Passing all of these, the code then creates (or gets, if it already exists), a page with the details passed, obtains a list of all the pages associated with the given category and calls `render()` with a new `page_listing.html` template.

Within this new `rango/page_listing.html` template, we simply copy and paste the markup and template code within Rango’s `category.html` template that dealt with listing pages.