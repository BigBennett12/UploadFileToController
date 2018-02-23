## View
        
    @using (Html.BeginForm("Create", "ControllerName", FormMethod.Post, new { enctype = "multipart/form-data" }))
    {
        @Html.AntiForgeryToken()

        @Html.ValidationSummary(false, "", new { @class = "text-danger", style = "list-style-position: inside;" })

        @Html.EditorFor(model => model.FilePath, new { htmlAttributes = new { @class = "form-control", type = "file" } })
        
        // code below

    }
        
## Controller
        
    [HttpPost]
    [ValidateAntiForgeryToken]
    public ActionResult Create([Bind(Include = "PropertyOne,PropertyTwo,PropertyThree,Filepath")] ClassName objectName)
    {
        if (ModelState.IsValid)
        {
            if (Request.Files.Count > 0)
            {
                var file = Request.Files[0];

                if (file != null && file.ContentLength > 0)
                {
                    string fileName = Path.GetFileName(file.FileName);
                    string path = Path.Combine(Server.MapPath(@"~/Content/MainFolderName/SubFolderName/" + fileName));
                    file.SaveAs(path);
                    objectName.Filepath = $"~/Content/MainFolderName/SubFolderName/{fileName}";
                }
            }

            db.TableName.Add(objectName);
            db.SaveChanges();
            return RedirectToAction("ActionName", "ControllerName", new { /* route values */ });
        }

        return View(objectName);
    }
