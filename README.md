# FSharp.Core API documentation generation

https://fsharp.github.io/fsharp-core-docs

## Contributing to Library Content

To improve the content of the F# Core library documentation, contribute to the XML `///` documentation in the
signature files (`*.fsi`) in the FSharp.Core implementation.

* Fork and clone https://github.com/dotnet/fsharp locally, see below, as a subdirectory of your copy of `fsharp-core-docs`

* Contribute to [the FSharp.Core directory ](https://github.com/dotnet/fsharp/tree/master/src/fsharp/FSharp.Core)

* Use a local build, see below

* Submit work to  `main` branch of https://github.com/dotnet/fsharp

* Once accepted your work will be published through a rebuild here, so submit a dummy pull request here

## Contributing to Generation of API Docs

The docs are generated by using `fsdocs` tool from FSharp.Formatting.  If you want to improve the generation process:

* Contribute to the API Docs mode and/or HTML generator in [FSharp.Formatting.ApiDocs](https://github.com/fsprojects/FSharp.Formatting/tree/master/src/FSharp.Formatting.ApiDocs) and [the `fsdocs` tool](https://github.com/fsprojects/FSharp.Formatting/tree/master/src/FSharp.Formatting.CommandTool) 

* Use a local copy of these, see below, as a subdirectory of fsharp-core-docs

* Submit work to the `master` branch of https://github.com/fsprojects/FSharp.Formatting

* Once accepted the new tooling will be published through a rebuild here, so submit a dummy pull request here

## Contributing to Layout and Design

These pages are currently using [the default template of the FSharp.Formatting tools](https://github.com/fsprojects/FSharp.Formatting/blob/master/docs/_template.html)
with its small amount of corresponding [CSS and JavaScript](https://github.com/fsprojects/FSharp.Formatting/tree/master/docs/content)

See [FSharp.Formatting styling](https://fsprojects.github.io/FSharp.Formatting/styling.html) for information on styling for output generated by `fsdocs`.

This template is *not* the long term plan (unless it is improved enough).  We can and must improve the design. Please help with this, and please be bold.  

1. Adjust the template and CSS in `docs`.  Rebuild as before.  Your template will be used instead of the default template.

2. After you have identified fixes and improvements, contribute back to the default template of FSharp.Formatting, or submit your work here and we can assess that for you.  If your design is good enough it might become a default design choice for all F# libraries.

It's a secondary goal of this repo to have the default template(s) of FSharp.Formatting to be good enough for this site. This means whatever improvements you make should eventually get copied across back into FSharp.Formatting (and the duplicated template and styling will then likely be removed from this repo once it's no longer needed). If the design diverges to be a completely different look and feel then we can make several templates available in FSharp.Formatting with this as one of them.



## Build steps

Eventually the build will just be

    dotnet tool restore
    dotnet restore FSharp.Core
    dotnet fsdocs build

For now, we want to pick up the latest copies of FSharp.Formatting and FSharp.Core, and set you up to make contributions to these. So we ask you to clone local copies of these:

    (start in 'fsharp-core-docs')
    dotnet restore FSharp.Core

    (make 'fsharp-core-docs/fsharp' and 'fsharp-core-docs/FSharp.Formatting' )
    git clone https://github.com/dotnet/fsharp --depth 1 -b main
    git clone https://github.com/fsprojects/FSharp.Formatting --depth 1

    (build 'fsharp-core-docs/fsharp')
    pushd fsharp
    .\build -noVisualStudio
    popd

    (build 'fsharp-core-docs/FSharp.Formatting')
    pushd FSharp.Formatting
    .\build -t Build
    popd
    
Then do iterative development using:

    (from 'fsharp-core-docs')
    FSharp.Formatting\src\FSharp.Formatting.CommandTool\bin\Release\netcoreapp3.1\fsdocs.exe watch --sourcefolder fsharp  

## CI Pipeline

This repo is published via GitHub Actions. On each push to master, the docs are built, and the outputs (which are written to the `output` directory by fsdocs) are pushed to the `gh-pages` branch. This repo is configured to host using GitHub Pages from this branch, so once the generated files are pushed the update is nearly-instant.

To build the very latest and freshest docs using the latest `fsdocs` tooling the CI does this:

1. build dotnet/fsharp `feature/docs` branch (where we assume latest doc updates have been pushed)

2. builds `FSharp.Formatting` master branch

3. Uses that `FSharp.Formatting` tool to build the docs for the FSharp.Core built in step 1

