---
layout: page
title: "vscode extension pack"
parent: projects
nav_order: 2
permalink: /projects/extension-pack/info
has_toc: false
---

# VSCode Extension Pack

The extensions packaged in this bundle are idenfied below in alphabetical order.
[jump to parent repo](https://github.com/bryandodd/vscode-bdpack){: .btn }

[Notes](#notes) at the bottom of this document.

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/soates/Auto-Import/raw/v1.5.3/icon.png" alt="logo" width="60"></td><td><h2>Auto Import</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=steoates.autoimport">
<img src="https://vsmarketplacebadge.apphb.com/version/steoates.autoimport.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/steoates.autoimport.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/steoates.autoimport.svg"></a></td></tr>
</table>

Automatically finds, parses, and provides code actions and code completion for all available imports. Works with Typescript and TSX.

<span class="fs-2">[`steoates.autoimport` on github](https://github.com/soates/Auto-Import){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/formulahendry/vscode-auto-rename-tag/raw/master/images/logo.png" alt="logo" width="100"></td><td><h2>Auto Rename Tag</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-rename-tag">
<img src="https://vsmarketplacebadge.apphb.com/version/formulahendry.auto-rename-tag.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/formulahendry.auto-rename-tag.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/formulahendry.auto-rename-tag.svg"></a></td></tr>
</table>

Automatically rename paired HTML/XML tag, same as Visual Studio IDE does.

<span class="fs-2">[`formulahendry.auto-rename-tag` on github](https://github.com/formulahendry/vscode-auto-rename-tag){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/natanfudge/Auto-Using/raw/master/icon.png" alt="logo" width="60"></td><td><h2>Auto-Using for C#</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=Fudge.auto-using">
<img src="https://vsmarketplacebadge.apphb.com/version/Fudge.auto-using.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/Fudge.auto-using.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/Fudge.auto-using.svg"></a></td></tr>
</table>

Auto-imports and provides intellisense for references that were not yet imported in a C# file.

![Sample](https://github.com/natanfudge/Auto-Using/raw/master/newdemo.gif)

<span class="fs-2">[`Fudge.auto-using` on github](https://github.com/natanfudge/Auto-Using){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://raw.githubusercontent.com/aws/aws-toolkit-vscode/master/resources/aws-icon-128x128.png" alt="logo" width="60"></td><td><h2>AWS Toolkit</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=AmazonWebServices.aws-toolkit-vscode">
<img src="https://vsmarketplacebadge.apphb.com/version/AmazonWebServices.aws-toolkit-vscode.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/AmazonWebServices.aws-toolkit-vscode.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/AmazonWebServices.aws-toolkit-vscode.svg"></a></td></tr>
</table>

This particular extension has been indispensable to me. So much of my day is spent working with AWS resources, this tool makes it faster and easier for me to interact with resources without having to jump through hoops clicking through the AWS web console. It covers a lot of ground, and I've generally found Amazon's [AWS Toolkit for Visual Studio Code documentation](https://docs.aws.amazon.com/toolkit-for-vscode/latest/userguide/welcome.html?icmpid=docs_tookitvscode_console) to be quite helpful getting started and working through problems. 

As of version `1.37.0`, it covers:
* [AWS Explorer](https://marketplace.visualstudio.com/items?itemName=AmazonWebServices.aws-toolkit-vscode#ui-components-aws-expl)
  * API Gateway
  * [App Runner](https://docs.aws.amazon.com/toolkit-for-vscode/latest/userguide/using-apprunner.html)
  * CloudFormation stacks
  * [CloudWatch Logs](https://marketplace.visualstudio.com/items?itemName=AmazonWebServices.aws-toolkit-vscode#cloudwatchlogs)
  * Elastic Container Registry _(ECR)_
  * [EventBridge schemas](https://marketplace.visualstudio.com/items?itemName=AmazonWebServices.aws-toolkit-vscode#eventbridge)
  * [IoT explorer](https://docs.aws.amazon.com/toolkit-for-vscode/latest/userguide/iot-start.html)
  * Lambda functions
  * S3 explorer
  * [Step Functions](https://marketplace.visualstudio.com/items?itemName=AmazonWebServices.aws-toolkit-vscode#sfn-files)
* [CDK Explorer](https://marketplace.visualstudio.com/items?itemName=AmazonWebServices.aws-toolkit-vscode#ui-components-cdk-expl) _(Cloud Development Kit)_
* [Serverless Applications](https://marketplace.visualstudio.com/items?itemName=AmazonWebServices.aws-toolkit-vscode#sam-and-lambda) _(SAM)_
* [Elastic Container Service task definition files](https://marketplace.visualstudio.com/items?itemName=AmazonWebServices.aws-toolkit-vscode#ecs-files) _(ECS)_
* [Systems Manager](https://marketplace.visualstudio.com/items?itemName=AmazonWebServices.aws-toolkit-vscode#ssm-files)
* [`AWS:` Commands](https://marketplace.visualstudio.com/items?itemName=AmazonWebServices.aws-toolkit-vscode#aws-commands)
* [Experimental Features](https://marketplace.visualstudio.com/items?itemName=AmazonWebServices.aws-toolkit-vscode#experimental-features)

![Sample](https://raw.githubusercontent.com/aws/aws-toolkit-vscode/master/resources/marketplace/vscode/overview-aws-explorer.png)

<span class="fs-2">[`AmazonWebServices.aws-toolkit-vscode` on github](https://github.com/aws/aws-toolkit-vscode){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/HookyQR/VSCodeBeautify/raw/master/icon.png" alt="logo" width="60"></td><td><h2>Beautify</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify">
<img src="https://vsmarketplacebadge.apphb.com/version/HookyQR.beautify.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/HookyQR.beautify.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/HookyQR.beautify.svg"></a></td></tr>
</table>

Beautify `javascript`, `JSON`, `CSS`, `Sass`, and `HTML` in Visual Studio Code.

VS Code uses js-beautify internally, but it lacks the ability to modify the style you wish to use. This extension enables running [js-beautify](http://jsbeautifier.org/) in VS Code, _AND_ honouring any `.jsbeautifyrc` file in the open file's path tree to load *your* code styling. Run with  **F1** `Beautify` (to beautify a selection) or **F1** `Beautify file`.

For help on the settings in the `.jsbeautifyrc` see [Settings.md](https://github.com/HookyQR/VSCodeBeautify/blob/master/Settings.md)

<span class="fs-2">[`HookyQR.beautify` on github](https://github.com/HookyQR/VSCodeBeautify){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/aaron-bond/better-comments/raw/master/icon.png" alt="logo" width="60"></td><td><h2>Better Comments</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=aaron-bond.better-comments">
<img src="https://vsmarketplacebadge.apphb.com/version/aaron-bond.better-comments.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/aaron-bond.better-comments.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/aaron-bond.better-comments.svg"></a></td></tr>
</table>

The Better Comments extension will help you create more human-friendly comments in your code.  
With this extension, you will be able to categorise your annotations into:
* Alerts
* Queries
* TODOs
* Highlights
* Commented out code can also be styled to make it clear the code shouldn't be there
* Any other comment styles you'd like can be specified in the settings

![Annotated code](https://github.com/aaron-bond/better-comments/raw/master/images/better-comments.PNG)

<span class="fs-2">[`aaron-bond.better-comments` on github](https://github.com/aaron-bond/better-comments){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/alefragnani/vscode-bookmarks/raw/master/images/icon.png" alt="logo" width="60"></td><td><h2>Bookmarks</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=alefragnani.Bookmarks">
<img src="https://vsmarketplacebadge.apphb.com/version/alefragnani.Bookmarks.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/alefragnani.Bookmarks.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/alefragnani.Bookmarks.svg"></a></td></tr>
</table>

It helps you to navigate in your code, moving between important positions easily and quickly. _No more need to search for code._ It also supports a set of **selection** commands, which allows you to select bookmarked lines and regions between bookmarked lines. It's really useful for log file analysis.

Here are some of the features that **Bookmarks** provides:

* **Mark/unmark positions** in your code
* Mark positions in your code and **give it name**
* **Jump** forward and backward between bookmarks
* Icons in **gutter** and **overview ruler**
* See a list of all Bookmarks in one **file** and **project**
* **Select lines** and **regions** with bookmarks
* A dedicated **Side Bar**

<span class="fs-2">[`alefragnani.Bookmarks` on github](https://github.com/alefragnani/vscode-bookmarks){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/OmniSharp/omnisharp-vscode/raw/master/images/csharpIcon.png" alt="logo" width="60"></td><td><h2>C#</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp">
<img src="https://vsmarketplacebadge.apphb.com/version/ms-dotnettools.csharp.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/ms-dotnettools.csharp.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/ms-dotnettools.csharp.svg"></a></td></tr>
</table>

This extension provides the following features inside VS Code:

* Lightweight development tools for [.NET Core](https://dotnet.github.io).
* Great C# editing support, including Syntax Highlighting, IntelliSense, Go to Definition, Find All References, etc.
* Debugging support for .NET Core (CoreCLR). NOTE: Mono debugging is not supported. Desktop CLR debugging has [limited support](https://github.com/OmniSharp/omnisharp-vscode/wiki/Desktop-.NET-Framework).
* Support for project.json and csproj projects on Windows, macOS and Linux.

The C# extension is powered by [OmniSharp](https://github.com/OmniSharp/omnisharp-roslyn).

<span class="fs-2">[`ms-dotnettools.csharp` on github](https://github.com/OmniSharp/omnisharp-vscode){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/microsoft/vscode-docker/raw/main/resources/docker_blue.png" alt="logo" width="60"></td><td><h2>Docker</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker">
<img src="https://vsmarketplacebadge.apphb.com/version/ms-azuretools.vscode-docker.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/ms-azuretools.vscode-docker.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/ms-azuretools.vscode-docker.svg"></a></td></tr>
</table>

The Docker extension makes it easy to build, manage, and deploy containerized applications from Visual Studio Code. It also provides one-click debugging of Node.js, Python, and .NET Core inside a container.

<span class="fs-2">[`ms-azuretools.vscode-docker` on github](https://github.com/microsoft/vscode-docker){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/everettjf/vscode-filter-line/raw/master/images/icon.png" alt="logo" width="60"></td><td><h2>Filter Line</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=everettjf.filter-line">
<img src="https://vsmarketplacebadge.apphb.com/version/everettjf.filter-line.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/everettjf.filter-line.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/everettjf.filter-line.svg"></a></td></tr>
</table>

Filter line for current opening file by strings/regular expressions, generating the result in a new file.

* Filter line by input string (or not contain input string).
* Filter line by input regular expression (or not match input regular expression).
* Filter line by config file `filterline.json`(or `filterline.eoml`) in corresponding `.vscode` directory.

![byregex](https://github.com/everettjf/vscode-filter-line/raw/master/img/byregex.gif)

<span class="fs-2">[`everettjf.filter-line` on github](https://github.com/everettjf/vscode-filter-line){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/Axosoft/vscode-gitlens/raw/main/images/gitlens-icon.png" alt="logo" width="60"></td><td><h2>GitLens</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens">
<img src="https://vsmarketplacebadge.apphb.com/version/eamodio.gitlens.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/eamodio.gitlens.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/eamodio.gitlens.svg"></a></td></tr>
</table>

GitLens supercharges the Git capabilities built into Visual Studio Code. It helps you to visualize code authorship at a glance via Git blame annotations and code lens, seamlessly navigate and explore Git repositories, gain valuable insights via powerful comparison commands, and so much more.

<p align="center">
  <img src="https://raw.githubusercontent.com/eamodio/vscode-gitlens/main/images/docs/code-lens.png" alt="Git Code Lens" />
</p>

<span class="fs-2">[`eamodio.gitlens` on github](https://github.com/Axosoft/vscode-gitlens){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/golang/vscode-go/raw/master/media/go-logo-blue.png" alt="logo" width="60"></td><td><h2>Go</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=golang.Go">
<img src="https://vsmarketplacebadge.apphb.com/version/golang.Go.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/golang.Go.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/golang.Go.svg"></a></td></tr>
</table>

This extension provides many features, including `IntelliSense`,
`code navigation`, and `code editing` support. It also shows `diagnostics` as
you work and provides enhanced support for `testing` and `debugging` your
programs. 

<span class="fs-2">[`golang.Go` on github](https://github.com/golang/vscode-go){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/hashicorp/vscode-terraform/raw/main/assets/icons/terraform_logo_mark_light_universal.png" alt="logo" width="60"></td><td><h2>HashiCorp Terraform</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=HashiCorp.terraform">
<img src="https://vsmarketplacebadge.apphb.com/version/HashiCorp.terraform.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/HashiCorp.terraform.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/HashiCorp.terraform.svg"></a></td></tr>
</table>

The HashiCorp Terraform Visual Studio Code (VS Code) extension adds syntax highlighting and other editing features for <a href="https://www.terraform.io/">Terraform</a> files using the [Terraform Language Server](https://github.com/hashicorp/terraform-ls).

- Manages installation and updates of the [Terraform Language Server (terraform-ls)](https://github.com/hashicorp/terraform-ls), exposing its features:
  - Completion of initialized providers: resource names, data source names, attribute names
  - Diagnostics to indicate HCL errors as you type
  - Initialize the configuration using "Terraform: init" from the command palette
  - Run `terraform plan` and `terraform apply` from the command palette
  - Validation diagnostics using "Terraform: validate" from the command palette or a `validateOnSave` setting
- Includes syntax highlighting for `.tf` and `.tfvars` files -- including all syntax changes new to Terraform 0.12
- Closes braces and quotes
- Includes `for_each` and `variable` syntax shortcuts (`fore`, `vare`, `varm`)

<span class="fs-2">[`HashiCorp.terraform` on github](https://github.com/hashicorp/vscode-terraform){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/microsoft/vscode-hexeditor/raw/main/icon.png" alt="logo" width="60"></td><td><h2>Hex Editor</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=ms-vscode.hexeditor">
<img src="https://vsmarketplacebadge.apphb.com/version/ms-vscode.hexeditor.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/ms-vscode.hexeditor.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/ms-vscode.hexeditor.svg"></a></td></tr>
</table>

A custom editor extension for Visual Studio Code which provides a hex editor for viewing and manipulating files in their raw hexadecimal representation.

- Opening files as hex
- A data inspector for viewing the hex values as various different data types
- Editing with undo, redo, copy, and paste support
- Find and replace

![User opens a text file named release.txt and switches to the hex editor via command palette. The user then navigates and edits the document](https://raw.githubusercontent.com/microsoft/vscode-hexeditor/main/hex-editor.gif)

<span class="fs-2">[`ms-vscode.hexeditor` on github](https://github.com/microsoft/vscode-hexeditor){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/microsoft/vscode-jupyter/raw/main/icon.png" alt="logo" width="60"></td><td><h2>Jupyter</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=ms-toolsai.jupyter">
<img src="https://vsmarketplacebadge.apphb.com/version/ms-toolsai.jupyter.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/ms-toolsai.jupyter.svg"><br/>
<img src="https://vsmarketplacebadge.apphb.com/rating/ms-toolsai.jupyter.svg"></a></td></tr>
</table>

A VSCode [extension](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.jupyter) that provides basic notebook support for [language kernels](https://github.com/jupyter/jupyter/wiki/Jupyter-kernels) that are supported in [Jupyter Notebooks](https://jupyter.org/) today. Many language kernels will work with no modification. To enable advanced features, modifications may be needed in the VS Code language extensions.

The Jupyter Extension includes the `Jupyter Keymaps` and the `Jupyter Notebook Renderers` extensions by default. The Jupyter Keymaps extension provides Jupyter-consistent keymaps and the Jupyter Notebook Rendereres extension provides renderers for mime types such as latex, plotly, vega and the like. Both of these extensions can be disabled or uninstalled.

<span class="fs-2">[`ms-toolsai.jupyter` on github](https://github.com/Microsoft/vscode-jupyter){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/Azure/vscode-kubernetes-tools/raw/master/images/k8s-logo.png" alt="logo" width="60"></td><td><h2>Kubernetes</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=ms-kubernetes-tools.vscode-kubernetes-tools">
<img src="https://vsmarketplacebadge.apphb.com/version/ms-kubernetes-tools.vscode-kubernetes-tools.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/ms-kubernetes-tools.vscode-kubernetes-tools.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/ms-kubernetes-tools.vscode-kubernetes-tools.svg"></a></td></tr>
</table>

The extension for developers building applications to run in Kubernetes clusters
and for DevOps staff troubleshooting Kubernetes applications.

Works with any Kubernetes anywhere (Azure, Minikube, AWS, GCP and more!).

Features include:

* View your clusters in an explorer tree view, and drill into workloads, services,
  pods and nodes.
* Browse Helm repos and install charts into your Kubernetes cluster.
* Intellisense for Kubernetes resources and Helm charts and templates.
* Edit Kubernetes resource manifests and apply them to your cluster.
* Build and run containers in your cluster from Dockerfiles in your project.
* View diffs of a resource's current state against the resource manifest in your
  Git repo
* Easily check out the Git commit corresponding to a deployed application.
* Run commands or start a shell within your application's pods.
* Get or follow logs and events from your clusters.
* Forward local ports to your application's pods.
* Create Helm charts using scaffolding and snippets.
* Watch resources in the cluster explorer and get live updates as they change

<span class="fs-2">[`ms-kubernetes-tools.vscode-kubernetes-tools` on github](https://github.com/Azure/vscode-kubernetes-tools){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/microsoft/vscode-npm-scripts/raw/main/npm_icon.png" alt="logo" width="60"></td><td><h2>npm</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=eg2.vscode-npm-script">
<img src="https://vsmarketplacebadge.apphb.com/version/eg2.vscode-npm-script.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/eg2.vscode-npm-script.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/eg2.vscode-npm-script.svg"></a></td></tr>
</table>

This extension supports running npm scripts defined in the `package.json` file and validating the installed modules
against the dependencies defined in the `package.json`.

**Notice** The validation is done by running `npm` and it is not run when the modules are managed by `yarn`.

The `package.json` validation reports warnings for modules:

- that are defined in the package.json, but that are not installed
- that are installed but not defined in the package.json
- that are installed but do not satisfy the version defined in the package.json.

Quick fixes to run `npm` are provided for reported warnings.

![package.json validation](https://github.com/microsoft/vscode-npm-scripts/raw/main/images/validation.png)

<span class="fs-2">[`eg2.vscode-npm-script` on github](https://github.com/Microsoft/vscode-npm-scripts){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/ChristianKohler/NpmIntellisense/raw/master/images/icon.png" alt="logo" width="60"></td><td><h2>npm Intellisense</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=christian-kohler.npm-intellisense">
<img src="https://vsmarketplacebadge.apphb.com/version/christian-kohler.npm-intellisense.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/christian-kohler.npm-intellisense.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/christian-kohler.npm-intellisense.svg"></a></td></tr>
</table>

Visual Studio Code plugin that autocompletes npm modules in import statements.

![auto complete](https://github.com/ChristianKohler/NpmIntellisense/raw/master/images/auto_complete.gif)

<span class="fs-2">[`christian-kohler.npm-intellisense` on github](https://github.com/ChristianKohler/NpmIntellisense){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://cdn.vsassets.io/v/M195_20211109.3/_content/Header/default_icon_128.png" alt="logo" width="60"></td><td><h2>NuGet Package Manager</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=jmrog.vscode-nuget-package-manager">
<img src="https://vsmarketplacebadge.apphb.com/version/jmrog.vscode-nuget-package-manager.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/jmrog.vscode-nuget-package-manager.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/jmrog.vscode-nuget-package-manager.svg"></a></td></tr>
</table>

An extension for Visual Studio Code that lets you easily add or remove 
.NET Core 1.1+ package references to/from your project's `.csproj` or `.fsproj`
files using Code's Command Palette.

- Search the NuGet package repository for packages using either (partial
or full) package name or another search term.
- Add PackageReference dependencies to your .NET Core 1.1+ `.csproj` or
`.fsproj` files from Visual Studio Code's Command Palette.
- Remove installed packages from your project's `.csproj` or `.fsproj` files via
Visual Studio Code's Command Palette.
- Handles workspaces with multiple `.csproj` or `.fsproj` files as well as
workspaces with single `.csproj`/`.fsproj` files.

![Adding a Package](https://github.com/jmrog/vscode-nuget-package-manager/raw/master/images/add-package.gif)

<span class="fs-2">[`jmrog.vscode-nuget-package-manager` on github](https://github.com/jmrog/vscode-nuget-package-manager){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/ChristianKohler/PathIntellisense/raw/master/icon/path-intellisense.png" alt="logo" width="60"></td><td><h2>Path Intellisense</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=christian-kohler.path-intellisense">
<img src="https://vsmarketplacebadge.apphb.com/version/christian-kohler.path-intellisense.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/christian-kohler.path-intellisense.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/christian-kohler.path-intellisense.svg"></a></td></tr>
</table>

Visual Studio Code plugin that autocompletes filenames.

To use Path Intellisense instead of the default autocompletion, the following configuration option must be added to your settings:

```javascript
{ "typescript.suggest.paths": false }
{ "javascript.suggest.paths": false }
```

![IDE](https://i.giphy.com/iaHeUiDeTUZuo.gif)

<span class="fs-2">[`christian-kohler.path-intellisense` on github](https://github.com/ChristianKohler/PathIntellisense){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/PowerShell/vscode-powershell/raw/master/media/PowerShell_Icon.png" alt="logo" width="60"></td><td><h2>PowerShell</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell">
<img src="https://vsmarketplacebadge.apphb.com/version/ms-vscode.PowerShell.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/ms-vscode.PowerShell.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/ms-vscode.PowerShell.svg"></a></td></tr>
</table>

This extension provides rich PowerShell language support for [Visual Studio Code](https://github.com/Microsoft/vscode) (VS Code).
Now you can write and debug PowerShell scripts using the excellent IDE-like interface
that Visual Studio Code provides.

This extension is powered by the PowerShell language server,
[PowerShell Editor Services](https://github.com/PowerShell/PowerShellEditorServices).
This leverages the
[Language Server Protocol](https://microsoft.github.io/language-server-protocol/)
where `PowerShellEditorServices` is the server and `vscode-powershell` is the client.

<span class="fs-2">[`ms-vscode.PowerShell` on github](https://github.com/PowerShell/vscode-powershell){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/prettier/prettier-vscode/raw/main/icon.png" alt="logo" width="60"></td><td><h2>Prettier - Code Formatter</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode">
<img src="https://vsmarketplacebadge.apphb.com/version/esbenp.prettier-vscode.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/esbenp.prettier-vscode.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/esbenp.prettier-vscode.svg"></a></td></tr>
</table>

[Prettier](https://prettier.io/) is an opinionated code formatter. It enforces a consistent style by parsing your code and re-printing it with its own rules that take the maximum line length into account, wrapping code when necessary.

<p align="center">
  <em>
    JavaScript
    · TypeScript
    · Flow
    · JSX
    · JSON
  </em>
  <br />
  <em>
    CSS
    · SCSS
    · Less
  </em>
  <br />
  <em>
    HTML
    · Vue
    · Angular
  </em>
  <br />
  <em>
    GraphQL
    · Markdown
    · YAML
  </em>
</p>

<span class="fs-2">[`esbenp.prettier-vscode` on github](https://github.com/prettier/prettier-vscode){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/microsoft/vscode-python/raw/main/icon.png" alt="logo" width="60"></td><td><h2>Python</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=ms-python.python">
<img src="https://vsmarketplacebadge.apphb.com/version/ms-python.python.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/ms-python.python.svg"><br/>
<img src="https://vsmarketplacebadge.apphb.com/rating/ms-python.python.svg"></a></td></tr>
</table>

A VSCode [extension](https://marketplace.visualstudio.com/VSCode) with rich support for the [Python language](https://www.python.org/) (for all [actively supported versions](https://devguide.python.org/#status-of-python-branches) of the language: >=3.6), including features such as IntelliSense (Pylance), linting, debugging, code navigation, code formatting, refactoring, variable explorer, test explorer, and more!

The Python extension will automatically install the [Pylance](https://marketplace.visualstudio.com/items?itemName=ms-python.vscode-pylance) and [Jupyter](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.jupyter) extensions to give you the best experience when working with Python files and Jupyter notebooks. However, Pylance is an optional dependency, meaning the Python extension will remain fully functional if it fails to be installed. You can also [uninstall](https://code.visualstudio.com/docs/editor/extension-marketplace#_uninstall-an-extension) it at the expense of some features if you’re using a different language server.

<span class="fs-2">[`ms-python.python` on github](https://github.com/Microsoft/vscode-python){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://cdn.vsassets.io/v/M195_20211109.3/_content/Header/default_icon_128.png" alt="logo" width="60"></td><td><h2>Remote - Containers</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers">
<img src="https://vsmarketplacebadge.apphb.com/version/ms-vscode-remote.remote-containers.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/ms-vscode-remote.remote-containers.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/ms-vscode-remote.remote-containers.svg"></a></td></tr>
</table>

The Remote - Containers extension lets you use a Docker container as a full-featured development environment. Whether you deploy to containers or not, containers make a great development environment because you can:

* Develop with a consistent, easily reproducible toolchain on the same operating system you deploy to.
* Quickly swap between different, separate development environments and safely make updates without worrying about impacting your local machine.
* Make it easy for new team members / contributors to get up and running in a consistent development environment.
* Try out new technologies or clone a copy of a code base without impacting your local setup.

The extension starts (or attaches to) a development container running a well defined tool and runtime stack. Workspace files can be mounted into the container from the local file system, or copied or cloned into it once the container is running. Extensions are installed and run inside the container where they have full access to the tools, platform, and file system.

<span class="fs-2">[`ms-vscode-remote.remote-containers` on github](https://github.com/Microsoft/vscode-remote-release){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/Huachao/vscode-restclient/raw/master/images/rest_icon.png" alt="logo" width="60"></td><td><h2>REST Client</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=humao.rest-client">
<img src="https://vsmarketplacebadge.apphb.com/version/humao.rest-client.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/humao.rest-client.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/humao.rest-client.svg"></a></td></tr>
</table>

REST Client allows you to send HTTP request and view the response in Visual Studio Code directly.

<span class="fs-2">[`humao.rest-client` on github](https://github.com/Huachao/vscode-restclient){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://enginedesigns.gallerycdn.vsassets.io/extensions/enginedesigns/retroassembler/1.3.6/1609517756245/Microsoft.VisualStudio.Services.Icons.Default" alt="logo" width="60"></td><td><h2>Retro Assembler</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=EngineDesigns.retroassembler">
<img src="https://vsmarketplacebadge.apphb.com/version/EngineDesigns.retroassembler.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/EngineDesigns.retroassembler.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/EngineDesigns.retroassembler.svg"></a></td></tr>
</table>

Retro Assembler is a lightweight but powerful macro assembler that can be used to develop software for classic computers and game consoles built with various CPU types. With its portability and Visual Studio Code integration it makes assembly development a pleasant experience on most mainstream operating systems.

<span class="fs-2">[`EngineDesigns.retroassembler` website](https://enginedesigns.net/retroassembler){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/rubyide/vscode-ruby/raw/main/packages/vscode-ruby-client/images/ruby.png" alt="logo" width="60"></td><td><h2>Ruby</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=rebornix.Ruby">
<img src="https://vsmarketplacebadge.apphb.com/version/rebornix.Ruby.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/rebornix.Ruby.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/rebornix.Ruby.svg"></a></td></tr>
</table>

This extension provides enhanced Ruby language and debugging support for Visual Studio Code.

- Automatic Ruby environment detection with support for rvm, rbenv, chruby, and asdf
- Lint support via RuboCop, Standard, and Reek
- Format support via RuboCop, Standard, Rufo, Prettier and RubyFMT
- Semantic code folding support
- Semantic highlighting support
- Basic Intellisense support

<span class="fs-2">[`rebornix.Ruby` on github](https://github.com/rubyide/vscode-ruby){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/Tyriar/vscode-sort-lines/raw/master/images/icon.png" alt="logo" width="60"></td><td><h2>Sort Lines</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=Tyriar.sort-lines">
<img src="https://vsmarketplacebadge.apphb.com/version/Tyriar.sort-lines.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/Tyriar.sort-lines.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/Tyriar.sort-lines.svg"></a></td></tr>
</table>

Sort lines of text in Visual Studio Code. 

![Usage animation](https://github.com/Tyriar/vscode-sort-lines/raw/master/images/usage-animation.gif)

<span class="fs-2">[`Tyriar.sort-lines` on github](https://github.com/Tyriar/vscode-sort-lines){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/microsoft/vscode-mssql/raw/main/images/sqlserver.png" alt="logo" width="60"></td><td><h2>SQL Server (mssql)</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql">
<img src="https://vsmarketplacebadge.apphb.com/version/ms-mssql.mssql.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/ms-mssql.mssql.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/ms-mssql.mssql.svg"></a></td></tr>
</table>

An extension for developing Microsoft SQL Server, Azure SQL Database and SQL Data Warehouse everywhere with a rich set of functionalities, including:

* Connect to Microsoft SQL Server, Azure SQL Database and SQL Data Warehouses.
* Create and manage connection profiles and most recently used connections.
* Write T-SQL script with IntelliSense, Go to Definition, T-SQL snippets, syntax colorizations, T-SQL error validations and `GO` batch separator.
* Execute your scripts and view results in a simple to use grid.
* Save the result to json or csv file format and view in the editor.
* Customizable extension options including command shortcuts and more.

See [the mssql extension tutorial](https://aka.ms/mssql-getting-started) for the step by step guide.

See [the SQL developer tutorial](http://aka.ms/sqldev) to develop an app with C#, Java, Node.js, PHP, Python and R with SQL Server databases.

<img src="https://github.com/Microsoft/vscode-mssql/raw/main/images/mssql-demo.gif" alt="demo" style="width:480px;"/>

<span class="fs-2">[`ms-mssql.mssql` on github](https://github.com/Microsoft/vscode-mssql){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/rangav/thunder-client-support/raw/master/images/thunder-icon.png" alt="logo" width="60"></td><td><h2>Thunder Client</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=rangav.vscode-thunder-client">
<img src="https://vsmarketplacebadge.apphb.com/version/rangav.vscode-thunder-client.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/rangav.vscode-thunder-client.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/rangav.vscode-thunder-client.svg"></a></td></tr>
</table>

Thunder Client is a lightweight Rest API Client Extension for Visual Studio Code, hand-crafted by [Ranga Vadhineni](https://twitter.com/ranga_vadhineni) with simple and clean design. The source code is not open source. 

![](https://github.com/rangav/thunder-client-support/raw/master/images/thunder-client.gif)

<span class="fs-2">[`rangav.vscode-thunder-client` on github](https://github.com/rangav/thunder-client-support){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/wayou/vscode-todo-highlight/raw/master/assets/icon.png" alt="logo" width="60"></td><td><h2>TODO Highlight</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=wayou.vscode-todo-highlight">
<img src="https://vsmarketplacebadge.apphb.com/version/wayou.vscode-todo-highlight.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/wayou.vscode-todo-highlight.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/wayou.vscode-todo-highlight.svg"></a></td></tr>
</table>

Highlight `TODO`, `FIXME` and other annotations within your code.

![](https://github.com/wayou/vscode-todo-highlight/raw/master/assets/material-night-eighties.png)

<span class="fs-2">[`wayou.vscode-todo-highlight` on github](https://github.com/wayou/vscode-todo-highlight){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/rubyide/vscode-ruby/raw/main/packages/vscode-ruby/images/logo.png" alt="logo" width="60"></td><td><h2>VSCode Ruby</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=wingrunr21.vscode-ruby">
<img src="https://vsmarketplacebadge.apphb.com/version/wingrunr21.vscode-ruby.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/wingrunr21.vscode-ruby.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/wingrunr21.vscode-ruby.svg"></a></td></tr>
</table>

This extension provides improved syntax highlighting, language configuration, and snippets to Ruby and ERB files within Visual Studio Code. It is meant to be used alongside the [Ruby extension](https://marketplace.visualstudio.com/items?itemName=rebornix.Ruby).

<span class="fs-2">[`wingrunr21.vscode-ruby` on github](https://github.com/rubyide/vscode-ruby){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/vscode-icons/vscode-icons/raw/master/images/logo.png" alt="logo" width="60"></td><td><h2>vscode-icons</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=vscode-icons-team.vscode-icons">
<img src="https://vsmarketplacebadge.apphb.com/version/vscode-icons-team.vscode-icons.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/vscode-icons-team.vscode-icons.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/vscode-icons-team.vscode-icons.svg"></a></td></tr>
</table>

Bring icons to your [Visual Studio Code](https://code.visualstudio.com/) (**minimum supported version: `1.40.2`**)

![demo](https://raw.githubusercontent.com/vscode-icons/vscode-icons/master/images/screenshot.gif)

<span class="fs-2">[`vscode-icons-team.vscode-icons` on github](https://github.com/vscode-icons/vscode-icons){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/DotJoshJohnson/vscode-xml/raw/master/resources/xml.png" alt="logo" width="60"></td><td><h2>XML Tools</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=DotJoshJohnson.xml">
<img src="https://vsmarketplacebadge.apphb.com/version/DotJoshJohnson.xml.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/DotJoshJohnson.xml.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/DotJoshJohnson.xml.svg"></a></td></tr>
</table>

**Features:**
* [XML Formatting](https://github.com/DotJoshJohnson/vscode-xml/wiki/xml-formatting)
* [XML Tree View](https://github.com/DotJoshJohnson/vscode-xml/wiki/xml-tree-view)
* [XPath Evaluation](https://github.com/DotJoshJohnson/vscode-xml/wiki/xpath-evaluation)
* [XQuery Linting](https://github.com/DotJoshJohnson/vscode-xml/wiki/xquery-linting)
* [XQuery Execution](https://github.com/DotJoshJohnson/vscode-xml/wiki/xquery-script-execution)
* [XQuery Code Completion](https://github.com/DotJoshJohnson/vscode-xml/wiki/xquery-code-completion)

<span class="fs-2">[`DotJoshJohnson.xml` on github](https://github.com/DotJoshJohnson/vscode-xml){: .btn }</span>

---

<table cellspacing="0" cellpadding="0">
<tr><td width="60" style="text-align:center"><img src="https://github.com/redhat-developer/vscode-yaml/raw/main/icon/icon128.png" alt="logo" width="60"></td><td><h2>YAML</h2><br/>
<a href="https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml">
<img src="https://vsmarketplacebadge.apphb.com/version/redhat.vscode-yaml.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/installs/redhat.vscode-yaml.svg"> 
<img src="https://vsmarketplacebadge.apphb.com/rating/redhat.vscode-yaml.svg"></a></td></tr>
</table>

Provides comprehensive YAML Language support to [Visual Studio Code](https://code.visualstudio.com/), via the [yaml-language-server](https://github.com/redhat-developer/yaml-language-server), with built-in Kubernetes syntax support.

**Features:**
1. YAML validation:
    * Detects whether the entire file is valid yaml
    * Detects errors such as:
        * Node is not found
        * Node has an invalid key node type
        * Node has an invalid type
        * Node is not a valid child node
2. Document Outlining (<kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>O</kbd>):
    * Provides the document outlining of all completed nodes in the file
3. Auto completion (<kbd>Ctrl</kbd> + <kbd>Space</kbd>):
    * Auto completes on all commands
    * Scalar nodes autocomplete to schema's defaults if they exist
4. Hover support:
    * Hovering over a node shows description *if provided by schema*
5. Formatter:
   * Allows for formatting the current file
   * On type formatting auto indent for array items

<span class="fs-2">[`redhat.vscode-yaml` on github](https://github.com/redhat-developer/vscode-yaml){: .btn }</span>

---

## Notes:

Bracket pairing/colorization is now 'built-in' to VSCode. To enable:

**settings.json**
``` json
{
    "editor.bracketPairColorization.enabled": true,
    "editor.guides.bracketPairs":"active"
}
```