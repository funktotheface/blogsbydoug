---
title: "Blog from the terminal!"
date: 2025-07-24
draft: false
featuredImage: feature2.webp
---
![dore2](feature2.webp)
This Blog Post was created and deployed to neocities straight from my vscode terminal! I will make a more detailed post on the process soon!
I appended this line to the blog post directly from the terminal again!, deploiying to neocities is as simple as git push!
Being able to blog to neocities directly from my terminal is a cool novelty for sure, but its actually a by-product of the way I have set up my hugo site and wired it into neocities! 

Using the neocities API key generator, I was able to create a git repo for my website and push its contents to git hub, then using the API key inside github I was able to set up an auto deployment action for neocities! So now whenever I make any changes or add new content into my blog and push it to github, it automatically deploys the content to neocities!

One of the really cool things about the setup is that my sites entire project uses approx 600mb of space, but the actual rendered files in the public folder are the only ones that get deployed to github, which means I can have a pretty extensive theme setup and resources available in the repo, but when i push the site to neocities only a few mb of space is required to host the output! 

