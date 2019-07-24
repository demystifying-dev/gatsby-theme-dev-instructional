For a written version of this course, check out the [Gatsby docs](https://www.gatsbyjs.org/tutorial/building-a-theme/).

## Transcript 01

Instructor: 00:01 In an empty directory, create a package.json. Inside the package.json, you're going to set private to true. Then you're going to set up your workspaces. This is an array of folder names. We're going to have two packages in this workspace. The first one is going to be called gatsby-theme-events. The second one will be called site.

00:31 Next, we're going to create folders for our packages. The first one is going to be called gatsby-theme-events. It's going to need its own package.json. We'll also need site with a package.json.

00:48 Inside the package.json, we're going to set up our theme. We need a name. This one is going to call it gatsby-theme-events. It's going to have a version. That version will be 1.00Then it's going to need a license. We'll set the license. Use the MIT license for now.

01:12 Because this will be installed as a package, we have to provide a main file, which we'll mark as index.js. In order to resolve properly, that file does need to exist. For now, we're going to create an index.js. We just leave a little comment to let people know that it was left blank on purpose.

01:34 In the site package.json, we're going to set it to private because we're not actually going to publish this site as an npm package. We're also going to give it a name, which is going to be site. This is what we refer to when we use a workspace.

01:48 Next, we're going to set a version. That version will again be 1.00Then we will set a license. That license will be the MIT license. We're also going to want scripts here. For our scripts, we want to have a build script. This is going to run the gatsby build command. We want a develop script. This will run gatsby develop. Finally, a clean script. This runs gatsby clean.

02:23 We want these to be available in both our theme and our site. We can copy-paste this over to our theme. Next, we can open up the terminal. We're going to install dependencies. To do this, instead of using the standard yarn add, we're going to use yarn workspace and then the package name, site, and add.

02:50 We want Gatsby, React, and ReactDOM, as well as our theme. We're going to install gatsby-theme-events. The reason that we use the `@*` is because we want the workspace to pick up an unpublished theme.

````
yarn workspace site add gatsby react react-dom gatsby-theme-events@*
````

03:14 Now that the installation is complete, we can see that it is referencing gatsby-theme-events. If we run yarn workspaces info, we can see that the site is using the gatsby-theme-events from the workspace.

````
yarn workspaces info
yarn workspaces v1.17.3
{
  "gatsby-theme-events": {
    "location": "gatsby-theme-events",
    "workspaceDependencies": [],
    "mismatchedWorkspaceDependencies": []
  },
  "site": {
    "location": "site",
    "workspaceDependencies": [
      "gatsby-theme-events"
    ],
    "mismatchedWorkspaceDependencies": []
  }
}
Done in 0.11s.
````

03:33 Our theme needs to have Gatsby, React, and ReactDOM added as peer dependencies. We'll do yarn workspace gatsby-theme-events. Add with the -P to mark it as a peer dependency. React, ReactDOM, and Gatsby.

````
yarn workspace gatsby-theme-events add -P react react-dom gatsby
````

04:01 During development, we're going to use our theme as a regular Gatsby site. We'll also set it up as a development dependency using -D. Once these finish installing, we can check our package.json and see that we have Gatsby, React, and ReactROM set up as both peer dependencies and dev dependencies.

````
yarn workspace gatsby-theme-events add -D react react-dom gatsby
````

04:27 To make sure that everything's working, we can just go ahead and start up Gatsby in development mode. We'll start with the site, yarn workspace site develop. This will start our Gatsby server in develop.

````
yarn workspace site develop -H 0.0.0.0 -p 6206
````

04:42 We don't have any content here. It's going to be an empty site, but we can grab out localhost 8000. We can go out to our browser and paste this up. We can see that Gatsby's running. There's no pages yet. Again, it's just going to give us this 404, but we can see that it's working.

04:59 Let's do the same thing with the theme, so gatsby-theme-events run develop. Now we can see here's localhost 8000 again. If we come back out and refresh the page, again we get that 404. Gatsby's running. It just needs some content.

````
yarn workspace gatsby-theme-events develop -H 0.0.0.0 -p 6207
````

## Transcript 02

Add Static Data to a Gatsby Theme
Jason Lengstorf

gatsbyGatsby
>=2.13.1

Learn how to load data from MDX (or any data source) and ensure the necessary folders exist.

To do this, we'll need to add the data in a folder in the project and install two plugins: gatsby-source-filesystem and gatsby-transformer-yaml

In gatsby-config.js, both of those plugins will be defined in the plugin array and the data will be exposed through out our application through GraphQL.

For a written version of this course, check out the [Gatsby docs](https://www.gatsbyjs.org/tutorial/building-a-theme/).

Instructor: 00:00 To get started on our theme, we need some data. We're going to create a folder called Data, and inside of it, we're going to create events.yml. Inside here, we're going to add information about conferences. Each of these conferences has a name, location, a start date and end date, and a URL.

00:19 To read these, we need to install a few more dependencies. In the terminal, we're going to run yarn-workspace-gatsby-theme-events add-gatsby-source-file-system. This is so that we can load this events.yml. Then we're going to install gatsby-transformer-yaml so that we can turn that yaml into something that Gatsby can use.

````
yarn workspace gatsby-theme-events add gatsby-source-filesystem gatsby-transformer-yaml
````

00:50 As we are waiting for that install, we're going to create a gatsby-config.js. In this, we're going to set up module.exports. This is our gatsby-config object.

01:04 In the plug-ins array, we're going to add gatsby-source-file-system. For it's options, we want to point to this data folder that we just created. Next, we add gatsby-transformer-yaml, and we're going to pass in an option to set the type name to event.

01:35 With our plug-ins configured, we can start up a Gatsby development server and see the data that's been loaded. Start this with yarn-workspace-gatsby-theme-events-develop. This starts up the Gatsby development server. While it hasn't added anything to the page, it is going to give us our GraphQL.

01:55 When we open localhost 8000/underscore-underscore-underscore-graphql, we're looking at the graphical explorer. In here, we can come in and take a look. We can see that we've got an all event query. Let's grab that and the nodes, and for now, let's just get the name.

02:14 We can see that the data from our yaml file has now been loaded. If we continue checking boxes, we can see that we have all of that data coming out which is great. We're now ready to start building pages.

## Transcript 03

Create a Data Directory in Gatsby using the onPreBootstrap lifecycle

Learn how to architect the React components in your Gatsby theme to maximize reusability and extendability while minimizing maintenance and hassle.

The first step is to make sure the data directory exists so because gatsby-source-filesystem will throw an error if it doesn't.

This lesson will cover that step.

Instructor: 00:00 To create pages, we need to create a file called gatsbynode.js. Inside our Gatsby node, we need to do several things. First, we need to make sure the data directory exists. The reason for that is that when we first fire up our theme, if that data directory doesn't exist, Gatsby's source file system is going to throw an error.

00:23 Second, we need to define the event type. The reason for that is that if we don't have any events to find, we should get an empty array not an error. Once that's done, we need to define resolvers for any custom fields. In this case, we're going to have a slug field.

00:45 Finally, we need to query for events and create pages. Our first step is going to be to make sure the data directory exists. To do this, we're going to use the onPreBootstrap API hook from Gatsby.

01:03 We're going to grab the reporter out to let people know that things are happening. Inside of this, we're going to decide where our content lives. We'll call this content path. This is going to be data. That's the data directory that we created over here.

01:20 Next, we're going to use the FS dependency from Node. This is a built-in. We're just going to get FS by requiring FS. Then we want to check if the content path doesn't exist, we want to create it.

01:43 We'll start by sending out a message. We'll say reporter.info, and we are creating the content path directory. Then we use FS. We're going to use makedersync to create the content path.

02:04 Now whenever we fire up a site using this theme, the first thing that'll happen is it's going to check if the data directory exists, and if not, it'll create it.

## Tapescript 04

Set up to Create Data Driven Pages in Gatsby

To actually create pages, we'll need to:

    define the event type
    define resolvers for custom fields
    query for events

Once we do this, we'll be ready to finally display the data in a page which is covered in: Display Data in React Provided in Gatsby with GraphQL

Instructor: 00:00 Our next step is to use the sourceNodes API hook, which will give us an actions object. We're going to define our node type or our event node. We do that using the createTypes action. Inside of this, we need to set up an event type that's going to implement the node interface.

00:26 We don't want Gatsby to infer any fields. We want to make sure that each one of them is explicitly defined. We're going to need an ID. We're also going to need the name, which is going to be a string. We need a location, which is going to be a string. We need the start date.

00:47 You'll note if we look at the events, start date doesn't actually line up with startDate, but we do want to use that camelCase style. We'll deal with that in just a second. Because the start date is a date, we want to use Gatsby's date built-in. We're going to use date format like this.

01:07 To make sure that we can actually load the start date into this field despite the fields not lining up, we're going to use the proxy directive, which will let us say where to pull it from. In this case, it's going to be from start date.

01:22 We can do exactly the same thing for the end date by duplicating that whole line and swapping it out for end date. Finally, we want to get the URL, which is this string. This URL is the live website. We also need to be able to create a URL on our own website.

01:41 We'll do that using a slug. The slug is going to be a string. It is a required string, but you'll notice that we don't have a slug to find in this object. We're going to need to define one ourselves, which we'll do with this custom resolver.

01:57 Gatsby gives us a createResolvers API hook. That gives us a function called createResolvers. Inside this function, we are going to set up a base path. This is the URL base. In our case, it's going to be the site index.

02:17 Then we need to set up a function to actually create our slugs. We're going to call this slugify. This is going to accept a string, which will be in our case the name of the event. That is going to need to return a slugified string.

02:35 Our first step is to turn the name into a slug, which we'll do by creating a variable called slug. That's going to get the string, which we will then turn into a lowercase string. After we have the name converted to lowercase, we need to change any character that's not a letter or a number into a hyphen. We'll use a regular expression for that with the replace function.

02:59 Let's set up a character set using square brackets and say anything that's not -- that's what this caret at the beginning means -- a lowercase A through Z or the number zero through nine. We can take one or more of those. We're going to take all of them that we find, not just the first occurrence, but every occurrence in the string. We're going to turn it into a hyphen.

03:23 This gives us a lowercase hyphenated slug, but it might still have a leading or a trailing hyphen if it started with, say, a quote. We want to add another regular expression to correct that.

03:36 We'll check if it starts with a hyphen. That's what the caret hyphen here means or with a pipe if it ends with a hyphen, which is what hyphen dollar sign means. Again, we'll just do a plus sign in case there are two hyphens or something at the end. We'll find every occurrence in the string and replace that with an empty string, so basically remove it.

04:02 Now that we've got a slug, we can return that. We're going to include the base path first, add a slash and then the slug. You probably just noticed that the base path has a slash in it. We're leading with a slash and we're adding another slash here. That actually means this is going to be three slashes.

04:22 We're going to do one last thing to prevent over-slashing. We're going to look for a forward slash, any sequence of two or more forward slashes, which we're doing by escaping a forward slash here, escaping another one, and then checking for one or more of those. This is going to be two-plus forward slashes. We'll replace those with a single forward slash.

04:50 The reason we're doing that is that if we decide to change this base path or we set up from an option, which we'll do in another lesson in this course, you want to make sure that you can't accidentally break your slugs by adding too many slashes. This is just a real quick check to make sure that you're returning a properly slashed slug.

05:12 With our slugify function defined, we can create our resolvers, which we do with the createResolvers function. Pass that an object. We get into the event type, the slug field. Then we want to set a function to resolve it. That function gets the source, which would be in this case the event node, so the name, location, start date, etc. We want to return the slugified version of the event name.

05:47 We can test that this is working by running yarn workspace gatsby theme events develop. This will give us our GraphQL layer again, which we can see here at 8000/graphql. If we open up GraphQL again, we can go back to all event, look into our nodes. Now we can see the start date and end date have been switched up to camelCase and we have a slug available.

06:22 Let's open these up. Because we set the end date and start date to be date types, we also get these helper functions, where we can do things like say fromNow. Using fromNow, we're able to do things like relative time, like how far away an event is, or we can format the string with something like this, where it'll give us the long date, the day, and the long year or a short date.

````
yarn workspace gatsby-theme-events develop -H 0.0.0.0 -p 6207

http://0.0.0.0:6207/___graphql

query MyQuery {
  allEvent {
    edges {
      node {
        name
        endDate(formatString: "MMMM DD, YYYY")
      }
    }
  }
}
````

## Transcript 05

Create Data Driven Pages in Gatsby with GraphQL and createPages

To create pages in Gatsby, there are 4 steps needed before actually doing the thing:

* ensure `data` directory exists
* define the event type
* define resolvers for custom fields
* query for events

Now that these are done (see lesson 03 and lesson 04), let's create the `allEvent` query and loop through the result with `createPages`.

Instructor: 00:00 Our last step in Gatsby Node is to create pages both for the even previews and individual event pages. We're going to do that using the createPages API hook. This one, because we're going to be running a GraphQL query, is going to be async.

00:17 We're going to start up by getting the actions. We want GraphQL. Let's also grab that reporter. Set up a base path, which we will set up to be just a / right now. We'll build the pages at root. Let's set up our first page, which we'll do using the createPage action.

00:42 This one is going to live at the base path. The component we're going to use doesn't exist yet, but it's going to be called event. We'll put it at source/template/events.js. We'll create that component in just a second.

01:01 Next, we'll set up our GraphQL query for individual events, which will store and result. Result is going to be the result of the GraphQL query, which will await because it returns a promise. That query is going to be all event and we want to sort these by the start date. We're going to sort them in ascending order.

01:24 We're going to get the fields of start date and the order is going to be ascending. All we need back from this is for each event node, we're going to get the id and the slug. If anything goes wrong, it will show up in result.errors. We can check for that here.

01:49 If something does go wrong, we'll run reporter.panic, which will cause a builder and put out an error message. We'll say there was an error loading events, and show the actual error that showed up, so reporter.errors.

02:07 Just to make sure that nothing else happens, we'll return. If everything goes well, we're able to grab at our events, which we will extract by going to result.data.allEvent.nodes. It will loop through each event.

02:27 We're going to get the slug out, which is going to be at event.slug, and call the createPage action. The path will be the slug. The component doesn't exist yet, but it's going to live in the source/templates/event.js file.

02:54 To make sure that we can identify which event is which, we're going to send in the event id as context. That is going to be pulled out of the event object at .id.

03:06 Our last step to make sure that these pages build is to create the components. We need to create the event and the events components in source/template.

03:19 Let's create the source/template and we'll start with events. These aren't going to show any data yet. They're just going to ensure the things don't break. We'll call this one events template. It's going to give us what to do.

03:46 We'll export that as the default. Copy that. We're going to create another placeholder. This one is going to be called event template. This is going to be for single events. To test this, we can run yarn workspace Gatsby theme events develop. That will give us a local host 8000, or we can go check to see that we've got our event's page.

04:28 If we trigger for 404 to see what's being created, we can see the pages for dinosaur.js, React Rally delete developer and JSHeroes have all been created. Each of those says there will be event page.

````
http://localhost:6207/404
````

