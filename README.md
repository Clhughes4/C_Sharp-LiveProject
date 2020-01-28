# C_Sharp-LiveProject

## Introduction
For my final two weeks with the Tech academy I took part in building and designing a software application for a theator company to manage all of their website content, requiring no technical competency. Much of the website and front-end design had yet to be constructed, so much of the app wasnt visually appealing, and most of the work which had been completed was all on the back end. As a software developer I very early could see the importance and usefulness of the frontend and backend of a software program. It is important to mantain an eqaul balance with front and back end development because without each there is no way to pull data from the database or for that data to be presented to the user. There were times I came to a cross road or had no idea of how to proceed with the assigned task, but through the value of research and the utilization of team collaboration I conquered the obstacles in my path or the mind fog I experienced. This live experience working on a team helped me to further understand what the day and a life of a software developer truly is. 

Below are some of the stories I worked along with their descriptions aand some code snippets.

## Story -- Cleaning Up the Subcriber Class
- There was an issue when the subscriber index was set up and I had to make sure the string primary key passed in can be handeled properly.

### Edit View
- I had to modify the Edit view to display the User associated with the Subscriber, but not allow that User to be changed. At the same time making sure to display the first and the last name of the Subscriber to all the other views.

```cshtml
@model TheatreCMS.Areas.Subscribers.Models.Subscriber

@{
    ViewBag.Title = "Edit";
    ViewBag.User = Model.SubscriberPerson.FirstName + " " + Model.SubscriberPerson.LastName;
}

<h2>Edit:</h2>
<br />


@using (Html.BeginForm())
{
    @Html.AntiForgeryToken()
    
    <div class="form-horizontal">
        <h4>Subscriber</h4>
        <hr />
        @Html.ValidationSummary(true, "", new { @class = "text-danger" })
        @Html.HiddenFor(model => model.SubscriberId)
        <br />

        <!--dv for a drop down menu-->
    <div class="form-group">
        @Html.LabelFor(model => model.SubscriberPerson, htmlAttributes: new { @class = "control-label col-md-2" })
        <div class="col-md-10">
            <label class="font-weight-bold h5">@ViewBag.User</label>
        </div>
    </div>
    <br />
```

### Delete View

```cshtml
@model TheatreCMS.Areas.Subscribers.Models.Subscriber

@{
    ViewBag.Title = "Delete";
    ViewBag.User = Model.SubscriberPerson.FirstName + " " + Model.SubscriberPerson.LastName;
}

<h2>Delete:</h2>

<h3>Are you sure you want to delete this?</h3>
<div>
    <h4>Subscriber</h4>
    <hr />
    <dl class="dl-horizontal">
        <dt>
            <label class="font-weight-bold h5">@(ViewBag.User)</label>
        </dt>
        <br />
```

### Index View
- To present the whole Subscriber's name, I seperated the first and last into their own column

```cshtml
@model IEnumerable<TheatreCMS.Areas.Subscribers.Models.Subscriber>

@{
    ViewBag.Title = "Index";
}

<h2>Index</h2>

<p>
    @Html.ActionLink("Create New", "Create")
</p>
<table class="table">
    <tr>
        <th>
            @Html.DisplayNameFor(model => model.SubscriberPerson.FirstName)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.SubscriberPerson.LastName)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.SubscriberPerson.UserName)
        </th>


 @foreach (var item in Model)
    {
        <tr>
            <td>
                @Html.DisplayFor(modelItem => item.SubscriberPerson.FirstName)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.SubscriberPerson.LastName)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.SubscriberPerson.UserName)
            </td>
```
## Story -- Front End Subcriber Index: Creating the Dashboard
- Created the entire dashboard for the Subscriber to manage their account
### Controller
```cs
namespace TheatreCMS.Areas.Subscribers.Controllers
{
    public class DashboardController : Controller
    {
        private ApplicationDbContext db = new ApplicationDbContext();
        public ActionResult Index()
        {
            string id = User.Identity.GetUserId();
            Subscriber subscriber = db.Subscribers.Find(id);
            return View(subscriber);
        }
    }
}
```

### Subscriber View
- Used cards to hold the upcoming shows and used flexboxes to hold the the logged in subscribers info to the dashboard. The other flex box was used to hold the calender.

```cshtml
@using Microsoft.AspNet.Identity
@model TheatreCMS.Areas.Subscribers.Models.Subscriber

@{
    ViewBag.Title = "Index";
}

<h2>Welcome to the Subscriber Area!</h2>

<div class="d-flex mt-5">
    <div class="align-self-stretch bg-info text-white col-md-4 mr-1" style="height:402px; border-radius:6px">
        <div class="d-inline-flex flex-row">
            <img src="" class="img-fluid" />
            <h3 class="align-items-center">Upcoming Shows</h3>
        </div>
        <div class="card bg-secondary mt-4 mb-3" style="max-width: 27rem;">
            <div class="card-body">
                <ul>
                    <li>1</li>
                    <li class="overflow-hidden"></li>
                    <li>2</li>
                    <li class="overflow-hidden"></li>
                    <li>3</li>
                    <li class="overflow-hidden"></li>
                    <li>4</li>
                    <li class="overflow-hidden"></li>
                    <li>5</li>
                </ul>
            </div>
        </div>
    </div>
    <div class="d-flex flex-column">
        <div class="p-2 align-self-start bg-info text-white mr-1" style="height:200px; width:200px; border-radius:6px;">
            <div class="d-inline-flex">
                @using (Html.BeginForm())
                {
                    @Html.AntiForgeryToken()
                    <div class="form-group">
                        @Html.ValidationSummary(true, "", new { @class = "text-danger" })
                        @Html.HiddenFor(model => model.SubscriberId)
                        <h3>@User.Identity.GetUserName()</h3>
                        <br />
                            <p>@Model.SubscriberPerson.StreetAddress</p>
                            <p>@Model.SubscriberPerson.PhoneNumber</p>
                            <p>@Model.SubscriberPerson.Email</p>
                    </div>
                }
            </div>
        </div>
        <div class="p-2 align-self-end bg-secondary text-white mt-1 mr-1" style="height:200px; width:200px; border-radius:6px;">
            <div class="d-inline-flex">
                <img src="" class="img-fluid" />
                <h4 class="align-items-center">Manage Subscription</h4>
                <p></p>
            </div>
            <a href="#" class="btn btn-info mt-3">Button</a>
        </div>
    </div>
    <div class="d-flex flex-column">
        <div class="p-2 align-self-start bg-secondary text-white mr-1" style="height:200px; width:200px; border-radius:6px;">
            <div class="d-inline-flex">
                <img src="" class="img-fluid" />
                <h4 class="align-items-center">Manage Account</h4>
                <p></p>
            </div>
            <a href="#" class="btn btn-info mt-3">Button</a>
        </div>
        <div class="p-2 align-self-end bg-info text-white mt-1 mr-1" style="height:200px; width:200px; border-radius:6px;">
            <div class="d-inline-flex">
                <img src="" class="img-fluid" />
                <h4 class="align-items-center"></h4>
                <p></p>
            </div>
        </div>
    </div>
</div>
```

## Story -- Back End: Bind Drop-down List to Controller
- Had to get the variables from the form into the controller and then assign them to the proper attributes for the model within the controller.
### Part Controller

```cs
 public ActionResult Create()
        {
            ViewData["dbUsers"] = new SelectList(db.Users.ToList(), "Id", "UserName");

            ViewData["CastMembers"] = new SelectList(db.CastMembers.ToList(), "CastMemberId", "Name");

            ViewData["Productions"] = new SelectList(db.Productions.ToList(), "ProductionId", "Title");
            return View();
        }

        // POST: Part/Create
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see https://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Create([Bind(Include = "PartID,Production,Person,Character,Type,Details")] Part part)
        {
            int productionID = Convert.ToInt32(Request.Form["Productions"]);
            int castID = Convert.ToInt32(Request.Form["CastMembers"]);

            if (ModelState.IsValid)
            {
                var person = db.CastMembers.Find(castID);
                var production = db.Productions.Find(productionID);
                
                part.Production = production;
                part.Person = person;
                db.Parts.Add(part);
                db.SaveChanges();
                return RedirectToAction("Index");
            }

            return View(part);
        }

            return View(part);
        }

        // GET: Part/Edit/5
        public ActionResult Edit(int? id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
            Part part = db.Parts.Find(id);
            if (part == null)
            {
                return HttpNotFound();
            }
            Part person = db.Parts.Find(id);
            if (person == null)
            {
                return HttpNotFound();
            }
            Part production = db.Parts.Find(id);
            if (production == null)
            {
                return HttpNotFound();
            }
            ViewData["CastMembers"] = new SelectList(db.CastMembers.ToList(), "CastMemberId", "Name");

            ViewData["Productions"] = new SelectList(db.Productions.ToList(), "ProductionId", "Title");
            return View(part);
        }

        // POST: Part/Edit/5
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see https://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Edit([Bind(Include = "PartID,Production,Person,Character,Type,Details")] Part part)
        {

            int castID = Convert.ToInt32(Request.Form["CastMembers"]);
            int productionID = Convert.ToInt32(Request.Form["Productions"]);
            if (ModelState.IsValid)
            {
                var currentPart = db.Parts.Find(part.PartID);
                currentPart.Character = part.Character;
                currentPart.Type = part.Type;
                currentPart.Details = part.Details;

                ViewData["Productions"] = new SelectList(db.Productions.ToList(), "ProductionId", "Title");

                ViewData["CastMembers"] = new SelectList(db.CastMembers.ToList(), "CastMemberID", "Name");

                var production = db.Productions.Find(productionID);
                
                var person = db.CastMembers.Find(castID);
                
                currentPart.Production = production;
                db.Entry(currentPart.Production).State = EntityState.Modified;
                db.SaveChanges();
                currentPart.Person = person;
                db.Entry(currentPart.Person).State = EntityState.Modified;
                db.SaveChanges();
                db.Entry(currentPart).State = EntityState.Modified;
                db.SaveChanges();

                return RedirectToAction("Index");
            }
            return View(part);
        }
```

### Edit View

```cshtml
@using (Html.BeginForm())
{
    @Html.AntiForgeryToken()
    
<div class="form-horizontal">
    <h4>Part</h4>
    <hr />
    @Html.ValidationSummary(true, "", new { @class = "text-danger" })
    @Html.HiddenFor(model => model.PartID)

    <div class="form-group">
        @Html.LabelFor(model => model.Production, htmlAttributes: new { @class = "control-label col-md-2" })
        <div class="col-md-10">
            @Html.DropDownList("Productions", string.Empty)
            @Html.ValidationMessageFor(model => model.Production)
        </div>
    </div>

    <div class="form-group">
        @Html.LabelFor(model => model.Person, htmlAttributes: new { @class = "control-label col-md-2" })
        <div class="col-md-10">
            @Html.DropDownList("CastMembers", string.Empty)
            @Html.ValidationMessageFor(model => model.Person)
        </div>
```

### Create View

```cshtml
<div class="form-group">
            @Html.LabelFor(model => model.Production, htmlAttributes: new { @class = "control-label col-md-2" })
            <div class="col-md-10">
                @Html.DropDownList("Productions", string.Empty)
                @Html.ValidationMessageFor(model => model.Production)
            </div>
        </div>
        <div class="form-group">
            @Html.LabelFor(model => model.Person, htmlAttributes: new { @class = "control-label col-md-2" })
            <div class="col-md-10">
                @Html.DropDownList("CastMembers", string.Empty)
                @Html.ValidationMessageFor(model => model.Person)
            </div>
        </div>
```        
