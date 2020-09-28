# Live Projects

#### [C# Live Project](#c-sharp-live-project)
#### [Python Live Project](#python-live-project)

### Introduction
As a part of the Software Developer Boot Camp at [The Tech Academy](http://learncodinganywhere.com), I was fortunate enough to work on two separate Live Projects. The C# Live Project consisted of a 2-week Sprint which doubled as an internship for Prosper IT. This Sprint utilized the Agile/Scrum methodologies using Azure DevOps for management of the project. The project consisted of developing a website for a Theater Production Company in Portland, Oregon using Visual Studio 2019, MVC, and EF Frameworking. I contributed to the front and back end devolopment. Examples and code snippets can be seen [below](#c#-live-project).

The Python Live Project involved creating a web-based app from scratch using PyCharm, MVT, and SQLAlchemy. The recent pandemic gave way to an unprecedented increase in sales of the Nintendo Switch and subsequently Nintendo Switch games and accessories. My simple app allows the user to enter his/her games manually, but it also allows them to view a list of Nintendo Switch games using the REST API RAWG Video Games Database. Future plans for the app involve branching out from simply video games and adding on Nintendo accessories and 3rd Party accessories. Examples and code snippets can be seen [below](#python-live-project).
<br>
<br>
<br>

## C Sharp Live Project
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

## Python Live Project