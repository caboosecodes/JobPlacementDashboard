# C# Live Project
## Introduction
While working with a group of my peers, I participated in a 2 week sprint working on an interactive website for managing content and productions for a theater/acting company. 
The project was built using ASP.NET MVC and Entity Framework and is meant to be a CMS, content management service, for users who are not technically savvy.

While much of the site was already built, making sure my additions/changes conformed to the styleguide and didn't interfere with other people's work was
a great learning oppotunity. I worked on styling the Navbar and using a code first approach I created a model, Rental History, and worked on the 
Create, Edit and Index pages for the model.

## Styling the Navbar
Styled the existing Navbar according to story specifications with Bootstrap, CSS, and Font Awesome Icons.

![navbar](https://user-images.githubusercontent.com/70549003/131183164-433ecfa4-ecde-4470-83be-3f624b1c7a46.gif)

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

## Index Page
The index page displays all the entries in the database in a table built and styled using Razor syntax, Bootstrap, CSS and Font Awesome icons.

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

Also, I wrote logic to grey out text if the rentals where marked with a checkmark.

```cshtml
@if (item.RentalDamaged == false)
              {
                <span class="rental_history-index--w text-secondary">
                  @Html.DisplayFor(modelItem => item.DamagesIncurred)
                </span>
              }
              else
              {
                <span class="rental_history-index--w">
                  @Html.DisplayFor(modelItem => item.DamagesIncurred)
                </span>
              }
```

Using the Razor @Url.Action() method I linked to the edit, details, and delete pages when the hamburger button is clicked.

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

![indexlink](https://user-images.githubusercontent.com/70549003/131183212-dd5891eb-08c5-4f20-a246-7f069c745ec8.gif)

## Create and Edit Pages
The Create and Edit pages for the Rental History model  were modeled with Bootstrap and CSS while adhering to the styleguide.
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

![checkbox](https://user-images.githubusercontent.com/70549003/131181511-80d2d9a5-5daa-42d5-9e77-b94d7d0c3f03.gif)


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

## Other Skills learned
* The majority of the work accomplished had to do with the front end of the website, which increased my familiarity with Bootstrap, CSS and HTML.
* Learned more about Razor syntax and the intricacies of it's html helpers.
* Gained a greater understanding of Visual Studio and version control with Azure Dev Ops.
