# mezzanine-recipes

This plugin gives you a "Recipe" blog post type for your Mezzanine sites.

## Features

* Show your visitors what to cook
	* Embed an image of the meal
	* Provide an ingredient list
* Let your visitors add a comment and rating to recipes
* The usual stuff - ingredients, times, categories
* REST-API for external applications

## Installation

* Run `pip install -U https://github.com/tjetzinger/mezzanine-recipes/tarball/master` (or, if you want to hack on mezzanine-recipes, clone it and run `pip install -e path/to/repo`)
* Install the Mezzanine CMS
* Add `"mezzanine_recipes",` followed by `"tastypie",` on top of your `INSTALLED_APPS`
* Migrate your database with `python manage.py migrate mezzanine_recipes`
* Install fixtures with `python manage.py loaddata mezzanine_required.json`
* To enable Recipe-Blog and REST API put following code to your urls.py:

```python
from mezzanine.conf import settings
from tastypie.api import Api
from mezzanine_recipes.api import *

v1_api = Api(api_name='v1')
v1_api.register(CategoryResource())
v1_api.register(KeywordResource())
v1_api.register(AssignedKeywordResource())
v1_api.register(PostResource())
v1_api.register(RecipeResource())
v1_api.register(BlogPostResource())
v1_api.register(IngredientResource())
v1_api.register(WorkingHoursResource())
v1_api.register(CookingTimeResource())
v1_api.register(RestPeriodResource())
v1_api.register(CommentResource())
v1_api.register(RatingResource())

urlpatterns = patterns("",
    ("^api/", include(v1_api.urls)),
    ("^%s/" % settings.BLOG_SLUG, include("mezzanine_recipes.urls")),
    ...
```

## Usage

mezzanine-recipe provides the blog post type "Recipe". The Recipe blog post type represents a single recipe.

Create a Recipe blog post in the Mezzanine admin (naming it something like "Recipe").

## Creating Templates

### Recipe page

The template for a Recipe blog post is `templates/recipe/blog_post_detail.html`.

The Recipe object is available at `mezzanine_recipes.recipe`. It has the following properties:

* Periods and times: `WorkingHours`, `CookingTime`, `RestPeriod`
* Cooking info: `ingredients`, `portions`, `difficulty`, `categories`
* Text data: `title`, `summary`, `content`, `comments`, `ratings`

## To Do

* Add some tests
* Extend Recipe type to single steps
