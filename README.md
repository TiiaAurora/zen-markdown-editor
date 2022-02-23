# Zen MarkDown Editor

This is a browser markdown Editor built in Svelte-Kit. It is a simple but powerful editor that can be used to write in markdown and reuse this content in other projects. 

Nothing will be saved on our servers. The content is stored in the browser's local storage and as long as you don't clear this storage, you can edit and save your content even after you close the browser.

Clicking on the "eye" icon gives you a preview of the content without the markdown syntax.

Clicking on the "copy" icon copies the content to the clipboard. You can then reuse it for example in a blog post on Hashnode or any other service that supports markdown.

The editor was created with [EasyMDE](https://github.com/Ionaru/easy-markdown-editor), a markdown editor that is built on top of the [simple-markdown-editor](https://github.com/sparksuite/simplemde-markdown-editor/).

## To run your own version use locally


```bash
npm run dev

# or start the server and open the app in a new browser tab
npm run dev -- --open
```

## For webservers

To create a production version of your app:

```bash
npm run build
```

You can then preview the production build with `npm run preview`. 
To deploy it on Netlify, just give Netlify access to the repository and create a build. 
In under a minute Netlifiy will deploy the build to your website. You then just have to change the URL to your liking. 

### Why this project?

This app was created for the Hashnode and Netlify Hackathon. The assignment was to create an app running on the netlify servers. 
To see a live version of this app visit: 

Live Demo: https://zen-markdown-editor.netlify.app/