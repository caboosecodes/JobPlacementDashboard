# C# Live Project
## Introduction
While working with a group of my peers, I participated in a 2 week sprint working on an interactive website for managing content and productions for a theater/acting company. 
The project was built using ASP.NET MVC and Entity Framework and is meant to be a CMS, content management service, for users who are not technically savvy.

While much of the site was already built, naking sure my additions/changes conformed to the styleguide and didn't interfere with other people's work was
a great learning oppotunity. I worked on styling the Navbar and using a code first approach I created a model, Rental Histories, and worked on the 
Create, Edit and Index pages for the model.

## Styling the Navbar
Styled the existing Navbar according to story specifications with Bootstrap, CSS, and Font Awesome Icons.
![image](https://user-images.githubusercontent.com/70549003/131169030-69facc76-4815-47a1-9968-f6cc7058af12.png)

## Creating the Model
I wrote a class, Rental History, to be used as a model in a code first approach using Entity Framework.

```cs
namespace TheatreCMS3.Areas.Rent.Models
{
    public class RentalHistory
    {
        public int RentalHistoryId { get; set; }
        public bool RentalDamaged { get; set; }
        public string DamagesIncurred { get; set; }
        public string Rental { get; set; }
    }
}
```

## Create and Edit Pages
Style the Create and Edit pages for the Rental History model with Bootstrap while adhering to the styleguide.
### Create Page
![image](https://user-images.githubusercontent.com/70549003/131168670-450b4632-fdf5-455d-afb2-82c01e0752c5.png)

I wrote a Javascript function to change the label "Notes" to "Damages Incurred" when the checkboxed is checked.
```javascript
var label = document.getElementById("damages");
var checkbox = document.getElementById("isDamaged");

// changes label from Notes to Damages incurred on create page
checkbox.addEventListener("change", function () {
    if (this.checked == true) {
        label.innerHTML = "Damages Incurred";
    } else {
        label.innerHTML = "Notes";
    }
});
```

*** INSERT GIF HERE ***

### Edit Page
The edit page has the same styling as the Create page but a different Javaacript function was used to make sure the correct label displayed depending on the status of the checkbox.
```javascript
var label = document.getElementById("damages");
var checkbox = document.getElementById("isDamaged");

// loads the proper label depending if the checkbox is already checked
window.addEventListener("load", function () {
    if (checkbox.checked == true) {
        label.innerHTML = "Damages Incurred";
    } else {
        label.innerHTML = "Notes";
    }
});
```

## Index Page
Display all entries in the database in a custom table styled by using CSHTML, CSS, Bootstrap and Font Awesome Icons.
![image](https://user-images.githubusercontent.com/70549003/131171033-69ccda17-7ac5-4ed7-b704-3a144fde589b.png)

I wrote logic using Razor syntax in the Index.cshtml file to display red cricle X for a damaged item and a green circle checkmark for an undamaged item.
```cshtml
@if (item.RentalDamaged == true)
          {
            <td class="rental_history-index--td1">
              <i class="fas fa-times-circle text-danger pl-1 rental_history-index--icnsize"></i>
            </td>
          }
          else
          {
            <td class="rental_history-index--td1">
              <i class="fas fa-check-circle text-success pl-1 rental_history-index--icnsize"></i>
            </td>
          }
```
When hovering over the table rows a hamburger button appears, with working links.
*** INSERT GIF HERE ***

Using the Razor @Url.Action() method I linked to the edit, details, and delete pages.
```cshtml
<button class="rental_history-index--btn btn dropdown float-right" type="button" id="dropdownMenuButton" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
              <i class="fas fa-ellipsis-v"></i>
            </button>
            <div class="dropdown-menu" aria-labelledby="dropdownMenuButton">
              <a class="dropdown-item" href="@Url.Action("Edit", new { id = item.RentalHistoryId })">
                <i class="fas fa-edit"></i>  Edit
              </a>
              <a class="dropdown-item" href="@Url.Action("Details", new { id = item.RentalHistoryId })">
                <i class="fas fa-info-circle"></i>  Details
              </a>
              <div class="dropdown-divider"></div>
              <a class="dropdown-item text-danger" href="@Url.Action("Delete", new { id = item.RentalHistoryId })">
                <i class="fas fa-trash-alt text-danger"></i>  Delete
              </a>
            </div>
```

