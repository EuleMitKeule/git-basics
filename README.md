
# Git SCM

In this tutorial you will learn how to use the Git source control management software to collaborate on your projects.

## 1. Getting Started

Git is a **free and open-source** distributed version control system that is widely used in software development.
It allows multiple developers to work on the same codebase **simultaneously** and **collaborate** on changes to the codebase.

To use Git you will first need to install the **Git software** on your computer. You can [download](https://git-scm.com/download) it from the official Git website.

After the installation the `git` **command line interface** will be available in the terminal of your choice.

### Repository Initialization

> Note: For Unity the project should already be **created before** you initialize the Git repository.

To start off you can open a **terminal** in your **project directory** and initialize a new Git repository with the following command:

```sh
git init
```

You will notice that a new folder was created in your directory. The `.git` folder contains the data Git needs to keep track of the changes made to your repository. You can ignore this folder as changing its contents is not necessary and might mess with your project.

### .gitignore

The next step is to **add** a **new file** to your repository which should be called `.gitignore`. Notice the file does not have a name but only consists of a file extension. This is intentional. The `.gitignore` file is used to configure which files and folders **shouldn't be included** in your Git repository. For most project types including Unity projects you can download a **template** gitignore from the [github/gitignore](https://github.com/github/gitignore) repository.

### Staging and Commits

Now that we have configured the repository we are ready to **commit** for the first time.
A commit in Git is a set of file **additions, modifications and deletions** that we give a name (the commit message). The first commit we create will contain all the files that were created by Unity for our project.

**Before** we create the commit we first have to tell Git which files and folders should be **included**. We do this by **staging** / adding the files like this:

```sh
git add some-file.txt
```

Most of the time though we wan't to stage **all changes** to the project that were made since the last commit. To stage everything simply specify the current directory instead of a single file or folder:

```sh
git add .
```

Now we are ready to **commit** our changes:

```sh
git commit -m "Create the Unity Project"
```

> Note: Be sure to use **meaningful** commit message. If you are unsure on how to format commit messages take a look at the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/#summary) standard.

> Note: You might encounter a message or warning telling you to configure your **name and email address**. Be sure to configure the same email that you will use to signup at the hosting provider in step two. You can do this using the following commands:

```sh
git config --global "user.name" "<Firstname> <Lastname>"
git config --global "user.email" "<your@mail.address>"
```

## 2. Working with the remote

Congratulations! You now have a working **Git repository** on your machine and know how to commit changes to it.
However to be able to **work together** on the same repository, you need a so called `remote` that is accessible for every person in the team. The most convenient way to accomplish this is by using a **Git Hosting Service** like [GitHub](https://github.com), [GitLab](https://gitlab.com) or [Bitbucket](https://bitbucket.org).

You can use any provider that you like, but this tutorial will focus on **GitHub**.

### Hosting

To get started hosting your repository you will need to **create an account** with the provider of your choice.
Be sure to use the same email address during signup that you configured in step one.

After creating your account you need to **create a repository** for your project on the hosting website.
GitHub will then need you to provide some basic information about your repository, including a **repository name**, whether the repository should be **visible** to the public or not and whether to create a `README`, `LICENSE` and `.gitignore` file.

> Note: You **do not** want to create any files in your repository. It should be created completely **empty**.

After the repository is created successfully, we can go on and **link** our local repository with the remote.
To do this you use the following command:

```sh
git remote add origin https://github.com/<your_username>/<your_reponame>.git
```

### Synchronization

Now you should be able to **synchronize** your locally created commits with the remote. This is done by `pushing` and `pulling` with pushing meaning to make your commits available to the remote and pulling meaning to make commits on the remote available to you locally.

The first time we push to the remote is special, since we need to make our local branch track the remote branch:

```sh
git branch -M main
git push -u origin main
```

These two commands ensure that the `main` branch of the repository exists both locally and on the remote.

From now on every time you **push** or **pull** you can simply write:

```sh
git pull
```
```sh
git push
```

> Note: If during Git installation you chose a different **default branch name** than `main` you will need to use that name instead. The default branch name you configured on your computer should also be set in your account settings on GitHub. If you chose `main` or didn't change the default name, everything should be configured correctly already.

### Git GUI

Using the `git` command line interface is the most powerful way to work with Git, but often it is more convenient to use a **GUI software** to perform actions like committing, pushing, pulling or creating branches.

There are many good programs to choose from. Examples include but are not limited to [GitHub Desktop](https://desktop.github.com), [Atlassian SourceTree](https://www.sourcetreeapp.com) and [GitKraken](https://www.gitkraken.com).

If you feel unsure about using the CLI we definetely recommend using one of these programs.

> Note: Most IDEs and code editors including **Visual Studio** and **Visual Studio Code** come with GUI based Git integrations that might be enough for most actions.

## 3. Advanced Concepts

By now you should be able to work on your project simultaneously with other team members. However you will soon encounter that **collaborating on files** might lead to situations that can't be dealt with by the Git SCM automatically.

### Branches

To reduce the possibility of such situations to a minimum and to keep active work processes in a **contained environment** Git allows the user to create so called `branches`.

A branch is a **separate line of development** that is created from the main codebase. Developers can work on different branches simultaneously and merge their changes back into the main codebase when they are ready.
When you commit to a branch the commits will only exist on this branch, until a **merge** on to the **main branch** is performed.

We recommend using branches for your projects, except when the project is very small. Branches can be **created** and be **switched** between easily using Git GUI software. When you are done working on the branch you can either merge the branch directly into the main branch using the GUI, or you can create a `Pull Request` / `Merge Request` on the remote.

Most often branches are created for a specific **feature** that will be implemented. You might have branches that look like this:

```
feature/player-jump-mechanics
feature/enemy-particle-systems
fix/controls-not-working
```

A different approach that might be more suitable for smaller projects would be to create a single branch for each **developer** in the team.


### Merge Conflicts

Performing a **merge** of a branch or **pulling** commits might lead to a so called `merge conflict`. In this case Git was not able to determine which **origin of truth** should be used going forward. In such a case the developer will have to manually specify which version of the data or source code should be kept.

For conflicts in source code files, you can use your favorite code editor or IDE to **resolve the conflicts**.
You will be asked to accept the `incoming` or the `current` changes. The current changes are the current state of the branch / repository that is being merged into and the incoming changes are the ones being pulled / merged.

For **conflicts with Unity** files, there is a tool called `UnityYAMLMerge` that will try to automatically handle merge conflicts in **scenes** or **assets**. For Git to be able to use this tool you will need to configure it inside your `.gitconfig` file. The `.gitconfig` file was created during Git installation inside your **user folder**.

The configuration is done by adding this to the file's contents:

```
    [merge]
    tool = unityyamlmerge

    [mergetool "unityyamlmerge"]
    trustExitCode = false
    cmd = '<path to UnityYAMLMerge>' merge -p "$BASE" "$REMOTE" "$LOCAL" "$MERGED"
```

> Note: The `<path to UnityYAMLMerge>` will be different depending on your OS and **Unity installation**. On Windows it will most likely look like this: `C:\Program Files\Unity\Hub\Editor\<unity_version>\Editor\Data\Tools\UnityYAMLMerge.exe`

Unfortunately even with the Smart Merge tool configured, merge conflicts on scene or asset files can sometimes occur when you collaborate on Unity projects.

To avoid conflicts, it's best to **divide** the project into smaller scenes that can be worked on **independently**. Each team member should work on their **own scene** and avoid making changes to scenes that other members are working on.

### Final Tips

1. **Establish a clear workflow**: It's important to establish a clear workflow for how changes will be made and merged in the project. This can include guidelines for branching, code reviews, and communication between team members.

2. **Use a .gitignore file**: Unity projects can generate a lot of unnecessary files that can cause conflicts and bloat the repository. Using a .gitignore file can help exclude these files and keep the repository clean.

3. **Commit early and often**: It's a good practice to commit changes early and often, even if they're small. This helps prevent large, complex changes that can be difficult to merge.

4. **Pull frequently**: Before making changes, it's important to pull the latest changes from the repository. This helps prevent conflicts that can arise from changes made by other team members.

5. **Communicate**: Effective communication is key to preventing conflicts. Make sure to communicate with other team members about changes that are being made and any potential conflicts that may arise.
