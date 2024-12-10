# Service Resources User Portal

This repo contains configuration that builds the `service-resources` portal. The portal stores the data for the up to date Arcimoto technical documentation, provided by the Technical Writing department, published by this tool for access by onsite vehicle technicians, via a secured web portal.

## Details

This repo uses an AWS CloudFormation template to build a interconnected set of lambdas, Cognito user pool, IAM and roles and policies. It uses an S3 bucket configured to serve it's data as a website via a secured CloudFront deployment for the onsite vehicle technicians to access the up to date technical documentation.

- The CloudFormation template sets up the AWS infrastructure.
- The HTML structure in the `resources` folder sits in the s3 bucket, with the lambdas authenticating users against the cognito user pool for access control.
- The content in the `resources/docs` folder is a historical history of the documentation section of the service portal. The Technical Writing department exports that content and commits it as part of the process to push a new version live.
- The content in the `resources/firmware` folder is an initial push of the firmware modules, but is not updated as part of the current deploy process and should be treated as outdated and not used.

### Instances

https://service-resources.arcimoto.com (Production instance)

https://dev.service-resources.arcimoto.com (Dev Instance)

## Repo-man

Cord Slatton

## Contributing

### Git commit formatting

We use the `Angular git commit` style

Full (long) version:

```git commit template
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

Short version

```git commit template
<type>(<scope>): <subject>
```

See https://github.com/angular/angular/blob/master/CONTRIBUTING.md#-commit-message-format more details

VS Code extension that helps you to make the commit msg, no need to remember your scope, etc.:
https://marketplace.visualstudio.com/items?itemName=Jhecht.git-angular

We use the allowed `types` from https://github.com/angular/angular/blob/master/CONTRIBUTING.md#-commit-message-format

We use custom `scopes` per repository, see `scopes` below.

#### Scopes

- config
- root

## Service Resource Portal Content

The `docs` folder is built by The Technical Writing department from a `MadCap Flare` output using the `HTML5SideNav` MadCap Flare template style. That content is uploaded manually using s3Browser, configured on the Technical Operations user's (typically Larry Carey) local computer.

The `firmware` folder is updated automatically via a bitbucket CI pipeline from the `deployedFirmware` repo master branch.

### Publishing

The content in the Service Resources Portal comes from the `/resources/docs/HTML5SideNav` folder in this repository. When a merge is made to the `dev` or `master` branch the `Documentation` section of `dev.service-resources.arcimoto.com` or `service-resources.arcimoto.com` is updated.

#### How to Release (Git Flow)

1. Create a new feature branch from the `dev` branch that is named according to a reasonable stable naming schema. Our common naming schema includes the ticket prefix to link it in Jira and then a short relevant description. Branch names do not allow spaces. Example of Arcimoto common schema: `TEL-123-example-functionality-change`. If this schema does not work for you, create one that does and stick to it.
2. Commit the changes to the feature branch for the revision being published.
3. Push the feature branch to BitBucket
4. Create a Pull Request (PR) from the feature branch to `dev`
5. Satisfy the merge conditions on the PR to allow it to merge: approval of reviewers, etc.
6. Merge to `dev`
7. Allow repository pipeline to run for `dev` - this deploys the content to the [Services Resources Portal Dev](https://dev.service-resources.arcimoto.com), and notifies the concerned parties of the update
8. Review content on [Services Resources Portal Dev](https://dev.service-resources.arcimoto.com)
9. If content needs revision before release to production then make those revisions to the files and go back to step 2
10. If the content passes review at the Service Resources Poral Dev then make a PR in BitBucket from `dev` to `master`
11. Satisfy the merge conditions on the PR to allow it to merge: approval of reviewers, etc.
12. Merge to `master`
13. Allow repository pipeline to run for `master` - this deploys the content to the [Services Resources Portal](https://service-resources.arcimoto.com), and notifies the concerned parties of the update
14. Review content on [Services Resources Portal](https://service-resources.arcimoto.com) to make sure it matches the release

#### Setup

You need to be able to perform basic git actions of `clone` a repo, change branches, `commit`, `push` and `pull`. You have to install `git` itself first. `VSCode` can help you perform `git` actions easily with a GUI. If you have a preferred way of interacting with `git` you are comfortable with, feel free to use that.

1. Download and Install [git](https://git-scm.com/downloads) on Windows using all the default options
2. Download, Install and Open [VSCode](https://code.visualstudio.com/download)
3. Set Git Bash as your default terminal in VSCode:
    - `Ctrl + Shift + P` to open the VSCode command palette
    - Type "Select Default Profile" and select it from the palette
    - Select `Git Bash` from the list of terminal options
4. Open a command line terminal window within VSCode (View Menu -> Terminal or Ctrl + Alt + `)
5. [Setup an ssh key for use with BitBucket](https://support.atlassian.com/bitbucket-cloud/docs/set-up-an-ssh-key/)
6. Select the `Source Control` tab along the far left-hand icon based sidebar of VSCode (the git branch icon: it looks like three circles connnected by lines, hovering over the icon will tell you the name of the panel)
7. Select the `Clone Repository` button
8. Enter the Repository URL `git@bitbucket.org:arcimotocode1/service-resources-portal.git` in the field that drops down and hit enter.
9. This opens a file navigation window to select the location on your computer the repository files will download: navigate to an appropriate location and confirm.
10. Once the download is complete select the `Open Folder` button in the left-hand panel of VSCode and navigate to the location you selected in the previous step and confirm
11. Along the bottom panel of VSCode to the far left is the git branch icon (3 circles connected by lines) with the name of the branch you are on, it should start on the `master` branch. Click the word `master`, the VSCode command palette will open at the top of the window. Select `origin/dev` to check out a local copy of the `dev` branch. You need the `dev` branch locally: each feature branch for releasing an update branches from `dev`. See the `How to Release (Git Flow)` section above for more information.

##### MadCap Flare Output

The MadCap Flare output `HTML5SideNav` folder can be output anywhere on your computer but will need to end up within this repo on your computer at `resources/docs` to work correctly. The easiest way to set this up will be to output directly to `resources/docs` within this repo, so it replaces the old content with the new, at which point you can commit the changes. See the `How to Release (Git Flow)` section above for more information.

## Service Resource Portal Users

The login to the production and dev portals are each controlled by a Cognito user pool. IT manages the allowed users based on the list of employed technicians. Each user in a given pool (production or dev) has equal permissions - pure authentication.

The IT and Vehicle Maintenance departments maintain their own documentation about their processes for this system.