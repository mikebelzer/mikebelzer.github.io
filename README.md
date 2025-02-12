# Mike Belzer
## Software developer, Tinkerer, Meddler, and Bit Twiddler Extraordinaire

As an accomplished IT professional with extensive experience in software development across a wide range of technologies, I have honed my skills in C#, .NET, JavaScript, TypeScript, Java, SQL Server, Oracle, React, and Angular. My career spans applications development, data application services, business intelligence, system administration, conversions, business requirements analysis, implementations, and report management.
I also have the following qualities:
- Spell Awful 
    - Is that how you spell awful?
- Sesquipedalian 
    - If you use this word you are one.
- Highly Opinionated 
    - Which doesn't matter because I am always right! 
- Sarcastic 
    - No Way, not me!
- Take the path less travelled 
    - I hear that only 2.5% of the workforce are code monkeys.

## Quotes

- Me fail english?  That's unpossible! - Ralph Wiggum
- Oh, so they have the internet on computers now. - Homer Simpson
- The definition of genius is taking the complex and making it simple - Albert Einstein
    - Not making simple thing complex like everone likes to do. 
- Kate: You always claim Ignorance! - Alf: I don't know what you are talking about!
>Is the fact that most of my favorite quotes are from fictional characters a problem? 

## Technology
Like every techie out there, I love technology.  What's not to love? It can make our lives simpler.  It can help us to do more. It can make connections and access simpler.  There is a certain comfort in the consistancy technology provides. There is a constant change in technology. It can be hard to keep up. It can take so much of your time. It can expose your personal data to the world. It can lead to addiction and environmental issues. It can be the bane of your existence when it doesn't work properly. I hate technology.
> Like everyone, I have a love/hate relationship with Tech.
> The struggle to keep up is real.

## 2025-2-11 - .Net Aspire
I recently watched the keynote to the .Net 9 launch, and one of the new things they spoke about caught my eye - Aspire.  I was genuinely impressed by what I saw. However, how many times have you seen a technology and thought "This is great!" only to discover it was, well, sort of great, or so-so, or just plain awful? Therefore, I took the time to check this out. Would it be great or hate? Awesome or gruesome?  A real hit or a piece of ....  Anyhow, everything I did I learned from this [Video](https://www.youtube.com/watch?v=4ixWtXK7KzY) [Credit to Jeff Fritz]
First, getting this up and running is super easy, you may already have the workloads in place to get started and not even know it.
- Visual Studio Example
    - Launch Visual Studio Installer
    - Go to individual components
    - Search for "Aspire" 
    - If the box is checked you already have the workload
    - If not check the box and do the update

From there it is pretty easy.  You can use the Aspire Project Template (actually a solution template) to start a project with all the bells and whistles, or you can take a current project and add in the Aspire projects. 
##### What do you get from Aspire?
First, you get the AppHost project and this is really the heart of how it all works. It in effect containerizes and runs your othert projects.  All you have to do is add the other projects as references, like so:
```sh
  <ItemGroup>
    <ProjectReference Include="..\StarColonies.MessageService\StarColonies.MessageService.csproj" />
    <ProjectReference Include="..\StarColonies.QueueFeed\StarColonies.QueueFeed.csproj" />
    <ProjectReference Include="..\StarColonies.Web\StarColonies.Web.Server\StarColonies.Web.Server.csproj" />
  </ItemGroup>
```
But actually it is smarter than that.  If you add a project it will automajically (did I spell that right?) add the reference for you! (oohs and ahhs) Pretty cool, huh? 
You can see then how it puts these in the Program.cs of the AppHost.
```sh
builder.AddProject<Projects.StarColonies_Web_Server>("starcolonies-web-server");
builder.AddProject<Projects.StarColonies_QueueFeed>("starcolonies-queuefeed");
builder.AddProject<Projects.StarColonies_MessageService>("starcolonies-messageservice");
```
So now all you do is run/debug the AppHost project and it will host all the other projects. So far so good.  Another great feature is that it attaches the threads for you so that your breakpoints are hit. Without that it wouldn't be as usefull for a developer.   
`Just one Gotcha that I ran into so far.` My project had a ReactJS frontend. It did not like it to launch from the AppHost.  So I had to set up a profile in the solution to launch multiple projects the AppHost and the React project.

Second, you get another project ServiceDefaults.  No this is great, because it add telemetry to your project at no extra cost. (And if it's for free, it's for me!) I mean zero effort here.

When you launch it comes up to a nice simple web page that has a table that looks something like this:
##### Resources

| Type | Name | State | Start time | Source | Endpoints | Actions |
| ------ | ------ | ------ | ------ | ------ | ------ | ------ | 
| Project | starcolonies-messageservice | Running | 10:15:12 AM | StarColonies.MessageService.csproj | | |
| Project | starcolonies-queuefeed | Running | 10:15:11 AM | StarColonies.QueueFeed.csproj | https://localhost:7015/swagger, http://localhost:5016/swagger | |
| Project | starcolonies-web-server | Running | 10:15:13 AM | StarColonies.Web.Server.csproj | https://localhost:7228/swagger, http://localhost:5125/swagger | |

From here you can do so much. There links to swagger for your apis.  Under the actions column, you can stop, restart, go to the console logs, see the structured logs, view traces and metrics.  All included. 
Looking at the console log for the message service, it looks just like a normal console window you would expect to see with scrolling data:
```sh
2025-02-11T10:31:36       Worker running at: 02/11/2025 10:31:36 -05:00 http://mars.jpl.nasa.gov/msl-raw-images/proj/msl/redops/ods/surface/sol/00127/opgs/edr/ncam/NLA_408775183EDR_D0051576NCAM00313M_.JPG
2025-02-11T10:31:37 info: StarColonies.MessageService.Worker[0]
2025-02-11T10:31:37       Worker running at: 02/11/2025 10:31:37 -05:00 http://mars.jpl.nasa.gov/msl-raw-images/proj/msl/redops/ods/surface/sol/00127/opgs/edr/ncam/NLA_408771669EDR_D0051566TRAV00035M_.JPG
2025-02-11T10:31:38 info: StarColonies.MessageService.Worker[0]
2025-02-11T10:31:38       Worker running at: 02/11/2025 10:31:38 -05:00 http://mars.jpl.nasa.gov/msl-raw-images/proj/msl/redops/ods/surface/sol/00127/opgs/edr/ncam/NLA_408771601EDR_D0051560TRAV00035M_.JPG
```
Switching over to the Structured logs for the Queue Feed API there is a table showing the all the entries:

| Resource | Level | Timestamp | Message | Trace | Actions |
|  ------ | ------ | ------ | ------ | ------ | ------ | 
| starcolonies-queuefeed | Information | 10:36:52.066 AM | Message queue at { Count = 101 } | db6614d | |
| starcolonies-queuefeed | Information | 10:36:52.340 AM | Message queue at { Count = 100 } | 5236d3c | |
| starcolonies-queuefeed | Information | 10:36:52.344 AM | Message queue at { Count = 99 } | f866b91 | |
| starcolonies-queuefeed | Information | 10:36:53.096 AM | Message queue at { Count = 100 } | d78453b | |

Great Information. But, wait!  There is more! Just click one of the rows, and you get more details.  The Hex codes under the Trace column are links to the Trace information. Where you can find further breakdown of response times.  
#### The Conclusion of the matter
Is Aspire worth it?  Does it live up to the Hype?  Yes! It was easy to set up and comes with a lot of pluses, why wouldn't you use it?  
