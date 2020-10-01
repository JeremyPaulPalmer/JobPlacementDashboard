# Prosper IT Consulting Internship

* #### [C# Internship](#parsing-awards-table)
* #### [Python Internship](#template-inheritance)

### Introduction
As a part of the Software Developer Boot Camp at [The Tech Academy](http://learncodinganywhere.com), I was fortunate enough to work as an intern on two separate projects for Prosper IT Consulting. The C# project consisted of a 2-week Sprint which utilized Scrum/Agile methodologies and Azure DevOps for project management. The project consisted of developing a website for a Theater Production Company in Portland, Oregon using Visual Studio 2019, MVC, and EF Frameworking. I contributed to the front and back end devolopment. Examples and code snippets can be seen [below](#parsing-awards-table).

The Python project involved creating a web-based app from scratch using PyCharm, MVT, and SQLite. The recent pandemic gave way to an unprecedented increase in sales of the Nintendo Switch and subsequently Nintendo Switch games and accessories. My simple app allows the user to enter his/her games manually, but it also allows them to view a list of Nintendo Switch games using the REST API RAWG Video Games Database. Future plans for the app involve branching out from simply video games and adding on Nintendo accessories and 3rd Party accessories. Examples and code snippets can be seen [below](#template-inheritance).
<br>
<br>

## C Sharp Project
### <a id="parsing_awards_table">Parsing Awards Table</a>
Out of all the User Stories, this one was by far the most challenging and most rewarding. This was a prime example of both a Front End and a Back End User Story.The original block of code simply contained the Awards data hardcoded which meant that any and every change needed to be performed manually. A previous User Story had been completed which seeded the Awards Table in the database. My task was to write the logic needed to display this information dynamically. As you can see below, I utilized a foreach loop with multiple nested if and else statements to achieve my goal of parsing the existing Awards information. The use of C#, html, CSS, and Bootstrap completed the Front End portion.
```
@*AWARDS AND ACHIEVEMENTS - THIS WILL BE RENDERED ONCE MVC IS CREATED*@
        <div class="card-deck m-lg-5">
            <div class="card ab-card">
              <div class="card-body">
                <h5 class="card-title ab-card-red">AWARDS &amp; ACHIEVEMENTS</h5>

                @{ var previousYear = ""; }
                @* Created foreach loop to loop through items in the Awards table and display Awards.*@
                @foreach (var item in Model)
                {
                  if (previousYear != item.Year.ToString())
                  {
                      previousYear = item.Year.ToString();
                      <a class="card-subtitle font-weight-bold"><u>@Html.DisplayFor(modelItem => item.Year)</u></a><br />
                  }

                  <a class="card-text text-uppercase">@Html.DisplayFor(modelItem => item.Name) @Html.DisplayFor(modelItem => item.Type) FOR @Html.DisplayFor(modelItem => item.Category) </a> <br />

                  if (item.Recipient != null)
                  {
                      <p><a class="card-text font-weight-bold">@Html.DisplayFor(modelItem => item.Recipient) </a> - <a class="card-text font-italic">@Html.DisplayFor(modelItem => item.Production.Title)</a></p>
                  }
                  else
                  {
                      <a class="card-text font-italic">@Html.DisplayFor(modelItem => item.Production.Title)</a>
                  }

                  if (item.OtherInfo != null)
                  {
                      <p class="card-text">(@Html.DisplayFor(modelItem => item.OtherInfo))</p>
                  }
                  else
                  {
                      <p></p>
                  }
                }

              </div>
            </div>
        </div>
```
### <a id="two_birds_one_method">Two Birds, One Method</a>
Another User Story I chose involved seeding the Sponsors table and adding the Sponsors photos to the Photos table. I noticed that some of my predecessors had separated similar data into two methods. I chose to write one method for both as shown below.

```
//Created a new method to create Sponsor photos, convert Sponsor photos to bytes, add Sponsor photos to Photos table, and seed the Sponsors 
        //with the 5 existing sponsors from the live website.
        private void SeedSponsors()
        {
                //Create images
                string imagesRoot = Path.Combine(AppDomain.CurrentDomain.BaseDirectory + @"\Content\Images");

                Image image1 = Image.FromFile(Path.Combine(imagesRoot, @"Ellyn-Bye-Logo.png"));
                Image image2 = Image.FromFile(Path.Combine(imagesRoot, @"LogoRoundColor.png"));
                Image image3 = Image.FromFile(Path.Combine(imagesRoot, @"Ninkasi-white-text.png"));
                Image image4 = Image.FromFile(Path.Combine(imagesRoot, @"OCF-logo-in-white-with-tagline-lg.png"));
                Image image5 = Image.FromFile(Path.Combine(imagesRoot, @"RACC2.png"));

                //Convert images
                var converter = new ImageConverter();

                var photos = new List<Photo>
            {
                new Photo
                {
                    OriginalHeight = image1.Height,
                    OriginalWidth = image1.Width,
                    PhotoFile = (byte[])converter.ConvertTo(image1, typeof(byte[])),
                    Title = "Ellyn Bye"
                },
                new Photo
                {
                    OriginalHeight = image2.Height,
                    OriginalWidth = image2.Width,
                    PhotoFile = (byte[])converter.ConvertTo(image2, typeof(byte[])),
                    Title = "Cider Riot!"
                },
                new Photo
                {
                    OriginalHeight = image3.Height,
                    OriginalWidth = image3.Width,
                    PhotoFile = (byte[])converter.ConvertTo(image3, typeof(byte[])),
                    Title = "Ninkasi Brewing"
                },
                new Photo
                {
                    OriginalHeight = image4.Height,
                    OriginalWidth = image4.Width,
                    PhotoFile = (byte[])converter.ConvertTo(image4, typeof(byte[])),
                    Title = "The Oregon Community Foundation"
                },
                new Photo
                {
                    OriginalHeight = image5.Height,
                    OriginalWidth = image5.Width,
                    PhotoFile = (byte[])converter.ConvertTo(image5, typeof(byte[])),
                    Title = "Regional Arts & Culture Council"
                },
            };

            //Add photos to Photo table
            photos.ForEach(photo => context.Photo.AddOrUpdate(c => c.PhotoFile, photo));
            context.SaveChanges();

            //Create sponsors
            var sponsor = new List<Sponsor>
            {

                new Sponsor{ Name = "Ellyn Bye", LogoId = context.Photo.Where(photo => photo.Title == "Ellyn Bye").FirstOrDefault().PhotoId,
                Height = image1.Height, Width = image1.Width, Current = true, Link = "", },

                new Sponsor{ Name = "Cider Riot!", LogoId = context.Photo.Where(photo => photo.Title == "Cider Riot!").FirstOrDefault().PhotoId,
                Height = image2.Height, Width = image2.Width, Current = true, Link = "https://www.ciderriot.com", },

                new Sponsor{ Name = "Ninkasi Brewing", LogoId = context.Photo.Where(photo => photo.Title == "Ninkasi Brewing").FirstOrDefault().PhotoId,
                Height = image3.Height, Width = image3.Width, Current = true, Link = "https://ninkasibrewing.com", },

                new Sponsor{ Name = "The Oregon Community Foundation",
                LogoId = context.Photo.Where(photo => photo.Title == "The Oregon Community Foundation").FirstOrDefault().PhotoId,
                Height = image4.Height, Width = image4.Width, Current = true, Link = "https://oregoncf.org", },

                new Sponsor{ Name = "Regional Arts & Culture Council",
                LogoId = context.Photo.Where(photo => photo.Title == "Regional Arts & Culture Council").FirstOrDefault().PhotoId,
                Height = image5.Height, Width = image5.Width, Current = true, 
                Link = "https://racc.org", },

            };

            sponsor.ForEach(Sponsor => context.Sponsors.AddOrUpdate(c => c.Name, Sponsor));
            context.SaveChanges();
        }
```

### If You Click It, They Will...
In this case, not do anything. This was actually my first User Story and to be quite honest, a HUGE confidence booster. I came into the Live Project a bit nervous. The completion of a previous User Story had rendered a couple links inoperable. I was tasked with getting them back up and working again using Bootstrap. It took some trial and error, but I finally landed on the class you see below.
```
<p class="position-relative"> @* Added to bring links to the foreground *@
  @if (ViewContext.HttpContext.User.IsInRole("Admin"))
  {
    @Html.ActionLink("Edit", "Edit", new { id = Model.ProductionId }) @:|
  }
    @Html.ActionLink("Current Productions", "Current") |
    @Html.ActionLink("Back to List", "Index")
</p>
```

## Python Project
### <a id="template_inheritance">Template Inheritance</a>
Once you grasp and understand template inheritance, it is like manna from heaven. As part of a larger project with many other developers working on their respective apps, my Nintendo Switch App is part of another app that essentially displays my app and other apps. Extending from the base of the main app allowed me to change some content while leaving the rest as "default" from the base. This allowed me to keep the overall feel of the main app while still being able to customize my app.

    {% extends "base.html" %}

    {% load staticfiles %}

    {% block title %}Nintendo Switch App{% endblock %}

    {% block tab-icon %}<link rel="icon" href="{% static 'NSW_App/images/switch_icon.png' %}"{% endblock %}

    {% block stylesheets %}
        {{ block.super }}
        <link rel="stylesheet" href="{% static 'NSW_App/css/NSW_App_layout.css' %}">
    {% endblock %}

    {% block pagetop-css %}bg_home{% endblock %}

    {% block navbar %}
        <header>
            <nav class="nav-wrapper">
                <div class="logo">
                    <a href="{% url 'home' %}">
                        {% block nav-icon %}<img src="{% static 'NSW_App/images/switch_icon.png' %}"{% endblock %}
                    </a>
                </div>
                <ul class="menu">
                    <li><a href="{% block home-button %}{% url 'NSWAppHome' %}{% endblock %}">Home</a></li>
                    <li><a href="{% url 'NSWAppAbout' %}">About</a></li>
                    <li><a href="{% url 'NSWNewsSearchAPI' %}">Search Switch News</a></li>
                    <li><a href="{% url 'CurrentGamesIndex' %}">Current Games</a></li>
                    <li><a href="{% url 'FutureGamesIndex' %}">Upcoming Releases</a></li>
                    <li><a href="{% url 'UserLibrary' %}">User Library</a></li>
                </ul>
            </nav>
        </header>
    {% endblock %}

    {% block page-title %}Nintendo Switch App{% endblock %}
    {% block page-subtitle %}Your One Stop Switch Shop{% endblock %}

    {% block button1 %}{% endblock %}
    {% block button2 %}{% endblock %}
    {% block button3 %}{% endblock %}
    {% block button4 %}{% endblock %}
    {% block button5 %}{% endblock %}

    {% block appcontent %}{% endblock %}

    {% block footer %}{% include 'NSW_App/NSW_App_footer.html' %}{% endblock %}

    {%  block javascript %}{% endblock %}

### Model
In Django, Models utilize ORM and SQLite to create tables within the database. The following Model creates a table for Nintendo Switch games including Title, Genre, Release Date, User Rating, and more. Users can manually enter game information by using a form which is shown in the next [section](#form-template).

    from django.db import models

    USER_RATINGS = [
        (0, 0),
        (1, 1),
        (2, 2),
        (3, 3),
        (4, 4),
        (5, 5),
    ]

    ESRB_RATINGS = [
        ('Everyone', 'Everyone'),
        ('Everyone 10+', 'Everyone 10+'),
        ('Teen', 'Teen'),
        ('Mature 17+', 'Mature 17+'),
        ('Adults Only 18+', 'Adults Only 18+'),
        ('Rating Pending', 'Rating Pending'),
    ]

    GENRES = [
        ('Action', 'Action'),
        ('Adventure', 'Adventure'),
        ('Arcade', 'Arcade'),
        ('Board', 'Board'),
        ('Card', 'Card'),
        ('Casual', 'Casual'),
        ('Educational', 'Educational'),
        ('Fighting', 'Fighting'),
        ('Indie', 'Indie'),
        ('MMO', 'MMO'),
        ('Open World', 'Open World'),
        ('Platformer', 'Platformer'),
        ('Racing', 'Racing'),
        ('RPG', 'RPG'),
        ('Shooter', 'Shooter'),
        ('Sim', 'Sim'),
        ('Sports', 'Sports'),
        ('Strategy', 'Strategy'),
    ]

    class Game(models.Model):
        title = models.CharField(max_length=100, verbose_name='Game Title', db_column='game_title', blank=False, null=False, unique=True)
        publisher = models.CharField(max_length=50, verbose_name='Publisher', db_column='publisher', blank=True, null=True)
        ESRB_rating = models.CharField(max_length=20, verbose_name='ESRB Rating', db_column='ESRB', blank=True, null=True, choices=ESRB_RATINGS)
        genre = models.CharField(max_length=30, verbose_name='Genre', db_column='genre', blank=False, null=True, choices=GENRES)
        release_date = models.DateField(verbose_name='Release Date', db_column='release_date', blank=True, null=True)
        user_rating = models.IntegerField(verbose_name='User Rating', db_column='user_rating', choices=USER_RATINGS, blank=True, null=True)

        class Meta:
            db_table = 'games_tbl'
            verbose_name = 'Game'

### <a id="form_template">Form Template</a>
This form template allows the user to visualize what information he/she will be adding to the games table. The behind-the-scenes code that actually adds the data to the table is shown below in the [views](#views) section.

    {% extends 'NSW_App/NSW_App_base.html' %}

    {% load staticfiles %}

    {% block title %}Nintendo Switch App | Add Game{% endblock %}

    {% block page-title %}Nintendo Switch App{% endblock %}
    {% block page-subtitle %}Add Game to User Library{% endblock %}

    {% block button1 %}{% endblock %}
    {% block button2 %}{% endblock %}
    {% block button3 %}{% endblock %}
    {% block button4 %}{% endblock %}
    {% block button5 %}{% endblock %}

    {% block appcontent %}
    <div>
        <form class="text-center" method="POST">
            {% csrf_token %}
            {% for field in form.visible_fields %}
            <div class="form-group">
                {{ field.label_tag }}<br>
                {% if field.help_text %}
                <p>{{ field.help_text|safe }}</p>
                {% endif %}
                {{ field }}
            </div>
            {% endfor %}
            <button type="button"><a class="cancel" href="{% url 'UserLibrary' %}">Cancel</a></button>
            <button type="reset">Reset</button>
            <button type="submit">Save</button>
        </form>
    </div>
    {% endblock %}

### <a id="views">Views</a>
The Views portion of the Django architecture essentially renders what you want the user to see when the user performs an action. For example, when a user clicks on the Save button when adding a game, the Add_Game function is called and executed. This function adds game information to the table using SQLite.

    from django.shortcuts import render, redirect
    from django.contrib import messages
    from .model_form import AddGameForm
    from .models import Game

    def NSW_APP_Homepage(request):
        return render(request, 'NSW_App/NSW_App_home.html')

    def NSW_APP_About(request):
        return render(request, 'NSW_App/NSW_App_about.html')

    def NSW_News_API(request):
        return render(request, 'NSW_App/NSW_App_API.html')

    def Current_Games_Index(request):
        return render(request, 'NSW_App/NSW_App_Current_Games.html')

    def Future_Games_Index(request):
        return render(request, 'NSW_App/NSW_App_Future_Games.html')

    def Add_Game(request):
        if request.method == 'POST':
            form = AddGameForm(request.POST)
            if form.is_valid():
                form.save()
                messages.success(request, 'Game Added to Library!')
                return redirect("UserLibrary")
        else:
            form = AddGameForm()
        return render(request, 'NSW_App/NSW_App_form.html', {'form': form})

    def User_Library(request):
        user_games = Game.objects.all()
        content = {
            'user_games': user_games,
        }
        return render(request, 'NSW_App/NSW_App_Library.html', content)

    def Details(request, pk):
        game = Game.objects.get(pk=pk)
        context = {
            'game': game
        }
        return render(request, 'NSW_App/NSW_App_details.html', context)

### API
![Under Construction](/img/images.jpg "Pardon My Dust! Still Open During Construction!")
