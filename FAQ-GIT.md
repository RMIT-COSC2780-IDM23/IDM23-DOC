# Git & GitHub FAQ

This is a FAQ for the project assessments only (i.e., Pacman assessment projects).

For general questions about the course, refer to the other FAQs available under [Resources](https://edstem.org/courses/6081/resources).

As any FAQ page, this page is always "under construction". As we realize that some questions become common, we add them here. So, bookmark it and check it regularly, particularly when you have a doubt and you suspect it may have been answered before!

- [Git \& GitHub FAQ](#git--github-faq)
  - [Git, GitHub, what is that?](#git-github-what-is-that)
  - [How do I submit my project solution in my GIT repository?](#how-do-i-submit-my-project-solution-in-my-git-repository)
  - [How do I change the submission tag if I have already tagged one commit for submission?](#how-do-i-change-the-submission-tag-if-i-have-already-tagged-one-commit-for-submission)
  - [How do I update the tags in my local repo? I get rejection with "(would clobber existing tag)" message](#how-do-i-update-the-tags-in-my-local-repo-i-get-rejection-with-would-clobber-existing-tag-message)
  - [Is a tag the same as a release in GitHub?](#is-a-tag-the-same-as-a-release-in-github)
  - [Cannot clone or push to GitHub with my password credentials?](#cannot-clone-or-push-to-github-with-my-password-credentials)
  - [I get `Permission denied (publickey).` from GitHub](#i-get-permission-denied-publickey-from-github)
  - [I have committed to the remote repo but I am not listed as a "contributor", why?](#i-have-committed-to-the-remote-repo-but-i-am-not-listed-as-a-contributor-why)
  - [Commits not correctly associated to my GitHub account, why?](#commits-not-correctly-associated-to-my-github-account-why)

## Git, GitHub, what is that?

We will use proper (git) version control in all our programming projects. This is totally standard practice in the industry and you would have seen that in previous courses (SEF and SEPT at least). Said so, if you want to refresh or have a quick intro to it, here are two resources I found useful:

- [30' video on Git & GitHub](https://youtu.be/jG4Vs81kMlc).
- [GitHub Guides](https://guides.github.com/).
- [Git \& GitHub Tutorial & Reference @ Javatpoint](https://www.javatpoint.com/git).

There are lots more good resources on the web of course!
## How do I submit my project solution in my GIT repository?

You submit by **tagging the _exact_ commit that you want to submit and be marked**, using the exact name given in the assignment specification. We will ONLY marked tagged submissions and will ONLY mark the tagged commit. Students can have many commits, and branches, even commits after the deadline. We will mark ONLY what you submit.

Of course we do not have access to your local machine, so the tag has to be created locally or then _pushed_ into GitHub remote repo (see below for several guides). Your tag has to show under `tags`, for example if the tag asked for is `submission`:

![google-form](imgs/tags-github-gui.png)

Observe that a tag `submission` is:

- NOT the same as a tag called "`Submission`" (i.e., tags are case-sensitive);
- NOT the same as a _branch_ called "`submission`";
- NOT the same as a _commit message_ "`submission`"; and
- NOT the same as a _release_ called "`submission`".

A tag is a specific point in the repository history, the point you want to be used for marking. A branch, a comment, and a release are different things.

While you can tag from your IDE (e.g., VS Code) you can always resort to command line to first create the tag in your local repo and then push it to the remote:

```shell
$ git tag -a submission <hash of commit to tag>
$ git push origin submission
```

Check the remote has the tag where you wanted!

Note that _a tag name can only be used once_, so if you already have a tag `submission` and want to use that tag name on another commit (e.g., you have a better, more recent, commit for your solution), you first need to delete the existing tag; see the next question for that. :-)

- For basic information on tagging, check [here](https://git-scm.com/book/en/v2/Git-Basics-Tagging).
- To create, push, and view tags in GitHub Desktop, check [here](https://docs.github.com/en/desktop/contributing-to-projects/managing-tags).
- To tag via command line or via GitHub web interface, check [here](https://stackoverflow.com/questions/18216991/create-a-tag-in-a-github-repository).

Note that the timestamp of the _commit_ is the submission date.

## How do I change the submission tag if I have already tagged one commit for submission?

This will happen when you realize you have a better version to submit than the one you submitted/tagged before. To do that, you need to first _delete_ the existing tag from both your local repo and from the server:

```shell
$ git tag --delete <tagname>  # first delete tag in the local repo
$ git push origin :refs/tags/<tagname>  # then delete remote tag
```

It is important to use `:refs/tags` when deleting the remote tag, as otherwise you may delete the branch with the same name! The empty string to the left of the colon causes the remote reference to be deleted!

Once the tag has been fully deleted, so you can re-use it on another commit!

See this as well on how to _rename_ an existing tag:

![google-form](img/how-to-rename-tag.png)

More information on how to delete git tags [here](https://devconnected.com/how-to-delete-local-and-remote-tags-on-git/).


## How do I update the tags in my local repo? I get rejection with "(would clobber existing tag)" message

To fetch the tags in the remote (e.g., tags pushed by other collaborators), you can do:

```shell
$ git fetch --all --tags -f
```

This will fetch all the changes from the remote, but also all the tags. The `-f` option will replace an existing tag (e.g., `submission`) with the one in the remote, if any (instead of failing). This could come very handy when different collaborators tag and re-tag commits as they incrementally work on a solution.

## Is a tag the same as a release in GitHub?

No, a _tag_ is a _git concept_, whereas a `Release` is something about GitHub, beyond git itself. So, they are not synonymous.

A tag is a _pointer_ to a specific commit, that's all, you basically give a name to a specific commit. This is what we use to mark the commit that is meant to be submitted for marking.
## Cannot clone or push to GitHub with my password credentials?

As [per August 12th, 2021 GitHub post](https://github.blog/changelog/2021-08-12-git-password-authentication-is-shutting-down/), GitHub is not no longer accepting account passwords when authenticating Git operations, like cloning private repos or pushing changes. You should use **token-based authentication**, such as  personal access, OAuth, SSH Key, or GitHub App installation token.

So, if you were still using a password to authenticate your GitHub.com operations (something never recommended anyways if you are doing development), you must start using a [personal access token](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token) by August 13, 2021 via HTTPS (recommended) or [SSH key](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh) to start using a personal access token to avoid disruption.

For example, you can set-up your remote as follows (after you generated your token in GitHub):

```shell
$ git remote set-url origin https://<token>@github.com/<username>/<repo>
```

As explained [here](https://www.sobyte.net/post/2021-08/github-deprecates-passwords-for-git-operations/), tokens offer many advantages over password-based authentication:

* **Unique:** tokens are specific to GitHub and can be generated on a per-use or per-device basis.
* **Revocable:** tokens can be individually revoked at any time without the need to update unaffected credentials.
* **Limited:** tokens can be narrowed to allow only the access required by the use case.
* **Random:** tokens are not subject to dictionary types or brute force attempts that might be made with simpler passwords that users need to remember or enter periodically.

## I get `Permission denied (publickey).` from GitHub

If yo get this error

```shell
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.
```

it most probably mean you have not correctly set-up your ssh keys into GitHub. GitHub needs your ssh public key to know it's you who is trying to access the repo. Please check [here](https://docs.github.com/en/authentication/troubleshooting-ssh/error-permission-denied-publickey).

Setting up GitHub to access with token or ssh is fundamental to have a productive environment, as you will be pushing and pulling from GitHub a lot! :-)

## I have committed to the remote repo but I am not listed as a "contributor", why?

The two main reasons may be:

1. Your commit is in a branch and has not yet made it to the default (master/main) branch, therefore you did not technically contribute (yet).
2. Your local Git commit email isn't connected to your account; [connect it](https://docs.github.com/en/github/setting-up-and-managing-your-github-user-account/managing-email-preferences/setting-your-commit-email-address)!

Read [this GitHub page](https://docs.github.com/en/github/setting-up-and-managing-your-github-profile/managing-contribution-graphs-on-your-profile/why-are-my-contributions-not-showing-up-on-my-profile#common-reasons-that-contributions-are-not-counted) to understand more about why your commit is not yet counting as contributions.

## Commits not correctly associated to my GitHub account, why?

Please [check this](https://docs.github.com/en/github/committing-changes-to-your-project/troubleshooting-commits/why-are-my-commits-linked-to-the-wrong-user) and fix it so we can know the commit was *yous*. Otherwise we may get your contributions wrong and risk getting lower marks or delaying your marking.
