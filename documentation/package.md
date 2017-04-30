# biddle, package.json

## Required
A package.json file is required for publication and installation support by biddle. This file must be saved in the root directory of the application with the filename *package.json*.

## Mandatory Fields

* *name* - The name field must be a string of greater than 0 length.  This is the name of the application.  This value should be file system safe for all common file systems.  If this value is not file system safe biddle will transform this value into something that is safer, which could result in a conflict if an application exists locally or is requested via the *install* command with this safer name.
* *version* - The version field must be a string of greater than 0 length.
   - The *publish* command will throw an error if the application already exists with this name and version.
   - The *install* command will throw an error if the currently installed application is of this same version.
   - The *publish* command will not produce a **latest** named archive if the current version contains *alpha* or *beta*.
   - There are no other rules. Style your versions as random gibberish if you wish.

## Optionally Supported Fields
* *test* - A string value representing a command line instruction for running an application's user acceptance tests via the **test** command.
* *publication_variants* - The variants field must be an object. The variants allow custom alternate publications of an application.
   - Each property name in this object becomes part of a variant file name.  For example an application named *myApp* at version *1.2.3* with a variant named *production* would produce files named:
      * myApp_1.2.3.hash
      * myApp_1.2.3.zip
      * myApp_production_1.2.3.hash
      * myApp_production_1.2.3.zip
   - Each property in the publication_variants object must be assigned to a child object containing one or both properties:
      * *exclusions* - An array of files or directories to delete from the application variant
      * *tasks* - An array of command line tasks to execute in synchronous order
   - Example:

```
    "publication_variants": {
        "min" : {
            "exclusions": [
                "libb", "testb"
            ],
            "tasks"     : ["prettydiff minify myApp/lib"]
        },
        "production": {
            "exclusions": ["testb"]
        }
    }
```

## Recommended Fields
Although additional data fields are not currently consumed by biddle some additional data is recommended.  Support for this data may be added to biddle at a later time.

* *date* - A date field indicating the date of latest publication is recommended.  I prefer the format yyyymmdd as it increments from the largest and least precise unit to the most precise unit to form a single number unique to a given day.  Version data is often hard for users to keep track of, but dates tend to be much easier.  More so if the dates the follow a single format.
* *description* - A verbose and descriptive explanation is helpful once many applications are installed.  Forgetting the purpose of a given application is easy when there are many application's installed.
* *repository* - When applications are working to encourage open source participation announcing the application's code repository is helpful.
* *contact* - A contact point allows users to engage with application maintainers.  This could be anything from an email address to the address of a bug tracking system.
