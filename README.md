# Live Projects

#### [C# Live Project](#c#-live-project)
#### [Python Live Project](#python-live-project)
<br>

### Introduction
As a part of the Software Developer Boot Camp at [The Tech Academy](http://learncodinganywhere.com), I was fortunate enough to work on two separate Live Projects. The C# Live Project consisted of a 2-week Sprint which doubled as an internship for Prosper IT. This Sprint utilized the Agile/Scrum methodologies using Azure DevOps for management of the project. The project consisted of developing a website for a Theater Production Company in Portland, Oregon using Visual Studio 2019, MVC, and EF Frameworking. I contributed to the front and back end devolopment. Examples and code snippets can be seen [below](#c#-live-project).

The Python Live Project involved creating a web-based app from scratch using PyCharm, MVT, and SQLAlchemy. The recent pandemic gave way to an unprecedented increase in sales of the Nintendo Switch and subsequently Nintendo Switch games and accessories. My simple app allows the user to enter his/her games manually, but it also allows them to view a list of Nintendo Switch games using the REST API RAWG Video Games Database. Future plans for the app involve branching out from simply video games and adding on Nintendo accessories and 3rd Party accessories. Examples and code snippets can be seen [below](#python-live-project).
<br>
<br>
<br>

## C# Live Project
### Back End User Stories
#### **Parsing Awards Table**
Out of all the User Stories, this one was by far the most challenging and most rewarding. The original block of code simply contained the Awards data hardcoded which meant that any and every change needed to be performed manually. A previous User Story had been completed which seeded the Awards Table in the database. My task was to write the logic needed to display this information dynamically. As you can see below, I utilized a foreach loop with multiple nested if and else statements to achieve my goal of parsing the existing Awards information.
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
## Python Live Project