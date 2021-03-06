# CodePlexDiscussionDownload

**Note:** CodePlex website was shutdown and the required webpages are no longer available so this tool does not work anymore.


CodePlexDiscussionDownload is a small utility application written in C# to download HTML and data from CodePlex Discusssion Forums.  The app scrapes the CodePlex Discussions downloading static HTML files of the thread list and thread details pages as well as extracting raw data and saving in a JSON format.

CodePlex is shutting down but the initial announcement indicates that a project's Discussions forum will not be included in the archived content.  [It looks like CodePlex may include that content](https://codeplex.codeplex.com/wikipage?title=Moving%20CodePlex%20to%20read-only): 

> We've heard your feedback and are working to try and include discussions in that archive as well.

However, if they don't come through this utility can download the Discussions HTML and also extract some of the raw data to save in JSON format so that it can be used later (e.g. queries, expose via REST etc.).

## Usage
```
Usage: CodePlexDiscussionDownload:

        f:forums                List of CodePlex Forums to download.
        l:logfile               Name of the logfile.  Default value is 'codeplex_discussion_download.log'.
        o:outputDirectory               Output directory where files are written. Default value is 'output'.
        p:pageSize              The number of posts on a generated HTML page.  Default value is 100.
```
The name of the forum should align with the name of the project URL. For example, for Prism the forum should be compositewpf because the name of the proejct URL is actually http://compositewpf.codeplex.com.  Multiple forums can be specified.  So to download the Discussion Forums for Prism and Unity use the following command line:

```
CodePlexDiscussionDownload.exe -f Unity compositewpf
```

## Installation
Download and extract the [zip](https://github.com/randylevy/CodePlexDiscussionDownload/releases/download/v1.0.0/CodeplexDiscussionDownload.zip), then run the executable using a command window.

# Output
## Folder Structure
The app will create a directory in the outputFolder.  

* Each thread listing page will be saved in the form of index_0.html...index_99.html
* Details pages will be saved as an index.html in a subfolder corresponding to the ID of the discussion thread (e.g. /142241/index.html)
* The raw JSON data will be saved in a file in the form {forum}.json (e.g. compositewpf.json)

Here is a small example of a folder structure
```
C:.
├───output
│   ├───compositewpf
│   │   │   compositewpf.json
│   │   │   index_0.html
│   │   │   index_1.html
│   │   │   index_2.html
│   │   │   index_3.html
│   │   │   
│   │   ├───100423
│   │   │       index.html
│   │   │       
│   │   ├───137452
│   │   │       index.html
│   │   │       
│   │   ├───155429
│   │   │       index.html
│   │   │       
│   │   ├───155489
│   │   │       index.html
│   │   │       
```

## JSON Format
Here is a sample of the raw JSON data:

``` json
[{
	"Id": "657365",
	"DiscussionDate": "Aug 23, 2016",
	"Time": "10:01 AM",
	"Title": "Region navigate to NonShared view/viewmodel",
	"AuthorUsername": "gan_s",
	"Posts": [{
		"Id": "1481868",
		"DiscussionThreadId": "657365",
		"DateTime": "8/23/2016 9:52:56 AM",
		"Username": "gan_s",
		"Content": "\r\n        <div class=\"markDownOutput \">Hi, <br>\r\nI'm trying to create multiple tabs in a tabcontrol. Each tabItem will be View.xaml with a ViewModel. I have set the PartCreationPolicy for both the view and the viewmodel to be NonShared. My viewmodel implements INavigationAware. I have a TabControlRegionAdapter\r\n defined as well. <br>\r\n<br>\r\nHowever when I do a regionManager.RequestNavigate to the view it seems to create only 1 instance of the view and viewmodel. I'm not sure how I should get the view and viewmodel to create a new instance for every tab item I create. Any help here will be much\r\n appreciated. <br>\r\n<br>\r\nI'm using Prism 4.1.0.0 with MEF. <br>\r\n<br>\r\nThanks. <br>\r\n<br>\r\nGanesh<br>\r\n</div>\r\n        \r\n    "
	}, {
		"Id": "1481870",
		"DiscussionThreadId": "657365",
		"DateTime": "8/23/2016 10:01:59 AM",
		"Username": "gan_s",
		"Content": "\r\n        <div class=\"markDownOutput \">Never mind...resolved this by setting IsNavigationTarget to return false. <br>\r\n<br>\r\nThanks. <br>\r\n<br>\r\nGanesh<br>\r\n</div>\r\n        \r\n    "
	}]
},]
```
## Known Issues
If the Discussion tab does not display the list of posts (but instead a list of topics or something else entirely) then the download utility will not be able to locate the posts.  For example, http://slab.codeplex.com/discussions does not list any posts but instead lists 3 topics (Contributing, General, Using) and the HTML is not parsed correctly by the utility.  I have not seen many projects with this particular structure so I believe this to not affect a large number of projects. 

## Caveats
In addition to the raw JSON data, this app downloads the raw HTML generated by CodePlex.  Note that this HTML is still quite CodePlex specific.  e.g. links point to CodePlex, images and styles are not present, etc.  Because of this, the generated HTML is not suitable initially for deploying to another website.  However, with a little global search/replace the generated HTML can be converted.

## Example Site
You can find a site created from this app at https://randylevy.github.io/UnityCodePlexForum/ (after a bit of global search/replace cleanup).  The HTML source for the site can be found at https://github.com/randylevy/UnityCodePlexForum/ and, if you are inclined to clean up the CodePlex HTML, useful images and styles can be found in that repo: https://github.com/randylevy/UnityCodePlexForum/tree/master/assets.
