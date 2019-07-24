## Transcript 01

For a written version of this course, check out the [Gatsby docs](https://www.gatsbyjs.org/tutorial/building-a-theme/).

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

