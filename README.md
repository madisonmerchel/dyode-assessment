# dyode-assessment

<b> 1. Describe how you would make a line of text in a homepage section editable from theme customization and how you would access this in liquid. </b>

To create an editable line of text within an existing homepage section we would first need to access the code for our theme, then find the coordinating liquid file under 'Sections'. 

From here, we would place the div for this text where we would like it to appear in the section. {{section.settings.text}} will output the text the user enters into the theme customizer. 

```html
{% if section.settings.text != blank %}
	<div class="editable-text">
		{{ section.settings.text }}
	</div>
{% endif %}
```

We would then add the following to the {% schema %} at the bottom of the file so the user is given the option to edit their new line of text in the theme customizer.

```html
    {
      "type": "richtext",
      "id": "text",
      "label": {
        "en": "Text"
      },
      "default": {
        "en": "<p>Enter your editable text here.</p>"
      }
    }
```

<b>2. How would you add the collection featured image as a banner on the collection liquid template? </b>

To implement an image as a basic static collection banner, I would use the following snippet to check for the image and load the file from the Shopify Admin. 

```html
{% if collection.image %}
	<div class="collection-banner">
		{{ collection.image | img_url: 'medium'}}
	</div>
{% endif %}
```
A potential problem with this is that there is no collection title. In cases like this, I often see clients placing the title on the image itself in Photoshop, which ends up poorly transferring to mobile screens. 

A preferred method of mine would be to check for the image, and then apply it as a background image within a parent div. Now, we can place the collection title above it within that div. 

Not only does this have a cleaner appearance, but it allows for some more adjustments with CSS and can scale better to different screen sizes. 

```html
{% if collection.image %}
<div class="collection-banner">
  <div class="collection-banner__image"
    data-bgset="{% include 'bgset', image: collection.image %}"
    data-sizes="auto"
    data-parent-fit="cover"
    style="background-image: url('{{ collection.image | img_url: '300x300' }});">
  </div>
  
  <div class="collection-banner__title-wrapper">
    <h1 class="collection-banner__title">
      <span role="text">
          {{ collection.title }}
        </span>
     </h1>
  </div> 
  
</div>
{% endif %}
```

<b>3. Using liquid code and HTML, create a simple pagination container, "< 1 2 ... 5 >". </b>

Below is a snippet for a basic pagination container using HTML5 and liquid.

```html
<ul class="pagination">

{% unless paginate.previous.is_link %}
	<a>Previous</a>
	{% else %}
	<a href="{{paginate.previous.url}}">Previous</a>
{% endunless %}

<li class="pagination_pages">
	{{ 'general.pagination.current_page' | t: current: paginate.current_page, total: paginate.pages }}
</li>

{% unless paginate.next.is_link %}
	<a>Next</a>
	{% else %}
	<a href="{{paginate.next.url}}">Next</a>
{% endunless %}
</ul>
```

<b>4. Using liquid code, access the product named "Blue T-Shirt". Store the id, title, handle, price, url, and image in variables. </b>

Below is one way to assign the requested variables based on the product's title using liquid. 

```html
{% if product.title == 'Blue T-shirt" %}
		 
	{{% assign blueshirtId = 123 %}}
	{{% assign blueshirtTitle = Blue T-Shirt %}}
	{{% assign blueshirtHandle = blue-t-shirt %}}
	{{% assign blueshirtUrl = /products/blue-t-shirt %}}
	{{% assign blueshirtImage = 'blueshirtimage.jpg | asset_url %}}

{% endif %}
```

<b>5. Using liquid code, create a key:value array using the list below. Loop through the array. Upon key type, store the value in a variable to be used later:
fruit:apple
vegetable:carrot
cloth:t-shirt
denim:jeans</b>

Below is a simple snippet that loops through our array, and assigns the appropriate value to each key that we can use for later.

```html
{% assign testArray = "fruit, vegetable, cloth, denim" | split: ',' %}

{% for key in testArray %}
	{% if key == 'fruit' %}
		{% assign fruit = "apple" %}
	{% else %}
	{% if key == 'vegetable' %}
		{% assign vegetable = "carrot" %}
	{% else %}
	{% if key == 'cloth' %}
		{% assign cloth = "t-shirt" %}
	{% else %}
	{% if key == 'denim' %}
		{% assign denim = "jeans" %}
	{% endif %}
{% endfor %}
```
