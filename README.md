# Python Live Project
## Introduction
While at The Tech Academy, I participated in a 2 week sprint where I created and developed a recipe app in the Django Framework where a user can create, edit and delete recipes.

## CRUD Functionality
The Django Framework utilizes create, read, update and delete (CRUD) operations that are executed on database models. My web app, Recipe Maker, was developed with these operations in mind.

### Create
This code creates the Model for my Recipe Maker App, where a model represents a table in the database and the model's fields represent that table's database fields.
```python
class Recipe(models.Model):
    recipe_type = models.CharField(max_length=60, choices=TYPE_CHOICES)
    recipe_name = models.CharField(max_length=100, default="", blank=False, null=False)
    recipe_ingredients = models.TextField(default="Please write you're ingredients and amounts", blank=False)
    recipe_instructions = models.TextField(default='Please write your instructions here', blank=False)

    objects = models.Manager()

    def __str__(self):
        return self.recipe_name
```
The following code allows the user to create and save a recipe to the database.
```python
def create_recipe(request):
    recipe_form = RecipeForm(request.POST or None) 

    if recipe_form.is_valid():
        recipe_form.save()
        return redirect('Recipe_Maker')
    else:
        print(recipe_form.errors)
        recipe_form = RecipeForm()
    context = {
        'recipe_form': recipe_form,
    }
    return render(request, 'Recipe_Maker/Recipe_Maker_create.html', context)
```

### Read
These view functions are Python functions that take a web request and returns a web response which are displayed via a webpage.

This function displays a list of the items in the database.
```python
def list_recipes(request):

    # set up Pagination
    p = Paginator(Recipe.objects.all().order_by('recipe_name'), 5)
    page = request.GET.get('page')
    recipes = p.get_page(page)

    return render(request, 'Recipe_Maker/Recipe_Maker_display.html', {'recipes': recipes})
```

This function displays a selected item in detail.
```python
def recipe_details(request, pk):
    # pk is the primary key
    recipe = get_object_or_404(Recipe, pk=pk)
    return render(request, 'Recipe_Maker/Recipe_Maker_details.html', {'recipe': recipe})
```

### Update and Delete
This view function allows the user to update a selected item from the database.
```python
def recipe_update(request, pk):
    recipe = Recipe.objects.get(pk=pk)
    form = RecipeForm(request.POST or None, instance=recipe)

    if form.is_valid():
        form.save()
        return redirect('list_recipes')

    return render(request, 'Recipe_Maker/Recipe_Maker_update.html', {'recipe': recipe, 'form': form})
```

This view function allows the user to delete an item from the database and returns them to the page where all the items are listed.
```python
def delete_recipe(request, pk):
    recipe = get_object_or_404(Recipe, pk=pk)

    if request.method == "POST":
        recipe.delete()
        return redirect('list_recipes')

    return render(request, 'Recipe_Maker/Recipe_Maker_confirmDelete.html', {'recipe': recipe})
```

## Web Scraping
In adition to writing functions that perform the basic CRUD operations on a database, I was able to utilize Beautiful Soup, a Python library used for pulling data out of HTML and XML, to parse through HTML and display a recipe in my app.
```python
def bsoup(request):
    # Website being scraped
    page = requests.get('https://www.foodnetwork.com/recipes/food-network-kitchen/apple-pie-recipe-2011423')
    page.content
    soup = BeautifulSoup(page.content, 'html.parser')

    # Recipe Title
    header = soup.find_all('span', class_='o-AssetTitle__a-HeadlineText')[0].get_text()

    # Grabs the ingredients from the website being scraped
    ingredients = soup.find_all('span', class_='o-Ingredients__a-Ingredient--CheckboxLabel')

    list_ingredients = []
    # [1:] skips the first (index of 0) in the ingredients' list
    for i in ingredients[1:]:
        # get_text() strips the tags off the object and returns a string
        text = i.get_text()
        # add the string to list_ingredients
        list_ingredients.append(text)

    # Grabs the description from the website
    description = soup.find_all('li', class_='o-Method__m-Step')
    list_description = []
    for d in description:
        text = d.get_text()
        list_description.append(text)

    context = {
        'header': header,
        'ingredients': list_ingredients,
        'description': list_description,
    }
    return render(request, 'Recipe_Maker/Recipe_Maker_bsoup.html', context)
```

## Front End Development
I used DTL, Django Template Language, to "extend" HTML from a base HTML file to my other HTML files without having to re-write HTML. Also, I implemented Bootstrap, a popular CSS Framework, to style my Recipe Maker app.

I implmented pagination on the webpage that displays items in a list from the database.
```django
<nav aria-label="Page navigation example ">
  <ul class="pagination">

{% if recipes.has_previous %}
    <li class="page-item"><a class="page-link" href="?page=1">&laquo; First</a></li>
    <li class="page-item"><a class="page-link" href="?page={{ venues.previous_page_number }}">Previous</a></li>
{% endif %}

<li class="page-item disabled"><a href="#" class="page-link">
    Page {{ recipes.number }} of {{ recipes.paginator.num_pages }}
</a></li>

{% if recipes.has_next %}
    <li class="page-item"><a class="page-link" href="?page={{ recipes.next_page_number }}">Next</a></li>
    <li class="page-item"><a class="page-link" href="?page={{ recipes.paginator.num_pages }}">Last &raquo;</a></li>
{% endif %}
  </ul>
</nav>
```
![image](https://user-images.githubusercontent.com/70549003/126020463-f17601a3-64f2-4b58-97f9-248180974093.png)

I was able to implement Bootstrap in a Django form class instead of implementing it in a Django template file where HTML is written.
```django
class RecipeForm(forms.ModelForm):
    class Meta:
        model = Recipe
        fields = ('recipe_type', 'recipe_name', 'recipe_ingredients', 'recipe_instructions')

        widgets = {
            'recipe_type': forms.Select(attrs={'class': 'btn btn-light dropdown-toggle'}),
            'recipe_name': forms.TextInput(attrs={'class': 'form-control'}),
            'recipe_ingredients': forms.Textarea(attrs={'class': 'form-control'}),
            'recipe_instructions': forms.Textarea(attrs={'class': 'form-control'}),
        }
```

## Skills Acquired
* Compentency in Django.
  * Better understanding Django's MVT design pattern.
* Familiarity with Azure DevOps.
  * Working with branches and version control.
