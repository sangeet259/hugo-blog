# hugo-blog
Hosts the configuration of my hugo-blog
To create yours follow the steps at https://kshitij10496.github.io/posts/hugo-blog-setup/

The following content is attributed to [Kshitij](https://www.linkedin.com/in/kshitij10496/)
---
Just copy pasting, editing few things from there for redundancy.

### Requirements
- OS: macOS, Ubuntu - the steps can easily extend to other Unix-based OS
- Terminal + Shell - for executing all the commands
- Hugo (Read: [Installation](https://gohugo.io/getting-started/installing/))
- Git (Read: [Installation](https://www.atlassian.com/git/tutorials/install-git))
- Text Editor - to write blog content (I prefer [Visual Studio Code](https://code.visualstudio.com/download))
- Web Browser - for accessing GitHub and selecting themes

By the end of the post, you should have a blog, with at least one post, up and running on GitHub Pages.  
Let's dive in!

## Hugo

#### Step 1: Create a basic Hugo site
1. Open up the terminal.
2. Navigate to the directory where you want to store the blog locally.
3. Create a new hugo site: ```$ hugo new site blog```
4. Change to the newly created `blog` directory: ```$ cd blog```

#### Step 2: Setup GitHub and Git locally
*(Working directory: `blog`)*

##### GitHub
1. Create a new repository on GitHub naming it as `<your-github-username>.github.io`
(`sangeet259.github.io`, in my case).

##### Git
1. Initialise the `blog` directory with Git: `$ git init`
2. Add the `$ git submodule add https://github.com/<username>/<username>.github.io public`
3. A new `public` folder should be created within the `blog` directory.
4. Choose a Hugo Theme for your blog from [here](https://themes.gohugo.io/)
5. Add the theme as another Git submodule: `$ git submodule add <theme-git-url> themes/<themeName>`  
Ex: For the `coder` theme, use: `$ git submodule add https://github.com/luizdepra/hugo-coder/`

**LESSON:** Using `git submodules` empowers us to specify theme version for our blog and reduce the code duplication and maintenance.

*We have to deal with atleast **2** separate Git repositories in the same directory.*

We will track the final statically generated website in the `public` sub-directory and host it on GitHub. 
Ideally, this would contain the final HTML and CSS files. The `blog` directory should contain the Markdown content, 
static images and configuration files relevant to Hugo.

**BONUS:** It's your choice whether you want to publish this repo to GitHub or not. Personally, I pushed mine to this very repo .

#### Step 3: Creating Content
*(Working directory: `blog`)*

1. Customise the `config.toml` file according to your needs. Refer: [Sample](https://github.com/sangeet259/hugo-blog/blob/master/config.toml)
2. Create a new content file: `$ hugo new posts/hello-world.md`
3. A new markdown file named `hello-world.md` is created in `content/posts` directory with some metadata.
4. Write your heart out! :love:
5. Preview the website with the newly created draft `$ hugo serve -D`
6. Navigate to your new site at `http://localhost:1313/` (most probably).
7. When you are happy with your post, set the `draft` header in the file to `false`.
8. Generate the final static website: `$ hugo`
9.  Move to `public` directory and you will notice many newly created HTML/CSS files aka "Your Blog".

The blog is all ready to be the next viral thing on the internet! :sunglasses:  
In the next step, we commit the changes and push the `public` folder to GitHub.

#### Step 4: Workflow for Publishing Content
*(Working directory: `blog/public`)*

1. Staging the changes made: `$ git add .`
2. Commiting the changes: `$ git commit -m "<your-commit-message>"`
3. Pushing to GitHub repo: `$ git push origin master`
4. Enter your GitHub credentials when/if prompted.
5. Your blog should be up and running on `https://<your-github-username>.github.io` within a couple of minutes.
6. Congratulations! You made it. :clap:

# Creating new content

Now that you have setup your blog, creating new posts is a breeze!  
You simply need to follow the method mentioned in **Step 3** and **Step 4** from above.  
Pretty clean, isn't it?

TODO:
[] Custom Fonts
[] Some minimalist underlining style  

Cheers! :beer: