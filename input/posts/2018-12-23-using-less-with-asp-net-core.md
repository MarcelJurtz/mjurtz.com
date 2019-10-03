---
title: 'Using LESS with ASP.NET Core'
date: 2018-12-23T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2018/12/using-less-with-asp-net-core
categories:
  - development
tags:
  - programming
  - software
  - development
  - css
  - web
---

I have already written about the advantages of a CSS preprocessor like LESS in this post. Today I want to show you how you can easily implement support for such a preprocessor in your ASP.NET Core project. I will use Gulp to create the ability to write LESS files and convert them to CSS files so that they can be integrated into the ASP.NET Core application. Finally, I will automate the whole thing so that the LESS file is converted without manual interaction on every build.  

To convert from LESS to CSS I use Gulp. Gulp is a workflow automation tool for JavaScript. This allows me to implement the whole thing per npm into my ASP.NET core project. I don't have any node packages in use yet, so I need a new package.json file first. I don't create it with Visual Studio because I didn't get it to work correctly. You are welcome to try it, but if it doesn't work either, try the following:  

Start the setup wizard using npm init, and fill in the configuration as needed. Then I need the following dependencies, which will be added to the dev-dependencies section. After adding them. the area will look something like this:

```json
"devDependencies": { 
  "gulp": "3.9.1", 
  "gulp-less": "3.3.2", 
  "gulp-clean-css": "3.9.0" 
} 
```
 

Now I need to setup gulp using a new file called gulpfile.js: 

```javascript
var gulp = require("gulp"), 
    cleanCss = require("gulp-clean-css"), 
    less = require("gulp-less"); 
 

gulp.task("default", function () { 
    return gulp.src('Styles/*.less') 
        .pipe(less()) 
        .pipe(cleanCss({ compatibility:  'ie8' })) 
        .pipe(gulp.dest('wwwroot/css')); 
}); 
```
 

And that's about it, actually. If your LESS files are in a directory other than "Styles", you have to change the configuration accordingly. You can now start the conversion with the following command:  

 
```xml 
node_modules\.bin\gulp 
```
 

Now I would like to automate this, because I do not want to manually execute the conversion everytime I changed something. To achieve that, I edit the .csproj file of my project by hand and add the following command:  

 
```xml 
<Target Name="MyPreCompileTarget" BeforeTargets="Build"> 
  <Exec WorkingDirectory="$(ProjectDir)" Command="node_modules\.bin\gulp" /> 
</Target> 
```

That's it. That's it. The conversion process is now fully automated and you can define and use your LESS files without worrying about the conversion. 