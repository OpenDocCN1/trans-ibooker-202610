```
{
    var car = await GetOneCarAsync(id);
    if (car == null)
    {
        return NotFound();
    }
    return View(car);
}

[HttpPost("{id}")]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, Car car)
{
    await _serviceWrapper.DeleteCarAsync(id,car);
    return RedirectToAction(nameof(Index));
}
```

# Update the View Component

MenuViewComponent is currently using the data access layer and the nonasync version of Invoke. Make the following changes to the class:

```
using System.Linq;
using System.Threading.Tasks;
using AutoLot.Dal.Repos.Interfaces;
using AutoLot.Services.ApiWrapper;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.ViewComponents;

namespace AutoLot.Mvc.ViewComponents
{
    public class MenuViewComponent : ViewComponent
    {
        private readonly IApiServiceWrapper _serviceWrapper;
        public MenuViewComponent(IApiServiceWrapper serviceWrapper)
        {
            _serviceWrapper = serviceWrapper;
        }

        public async Task<IViewComponentResult> InvokeAsync()
        {
            var makes = await _serviceWrapper.GetMakesAsync();
            if (makes == null)
            {
                return new ContentViewComponentResult("Unable to get the makes");
            }
            return View("MenuView", makes);
        }
    }
}
```