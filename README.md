## My Artifacts Repository

This repository collects artifacts built from my GitHub projects and can be used as Maven repository
for Maven or Gradle build of projects depend on these artifacts.

### How to create repo like this

This document was inspired by
[How to create a maven repository for your github project step by step](https://gist.github.com/fernandezpablo85/03cf8b0cd2e7d8527063).
Unlike that document I suggest to create separate GitHub project to host all artifacts you publishing. This allows
to allow dependencies on all your projects by adding single repository. Also hosting artifacts in the separate branch
of the project with source make maintenance of branches more confusing.

Finally combining all projects artifacts into single project allows to host separate repositories
(libs/plugins and snapshot/release) in separate branches.

#### Create GitHub project

Create your own project in GitHub and check it out to local system.

Create separate branch for each repository. Artifactory creates by default follow repos:
1. libs-snapshots
1. libs-releases
1. plugin-snapshots
1. plugin-releases

Also each of these repos has "-local" and remote copy.

This makes sense, but anyone may create any other (or not create any of these) repos.

#### Publish artifact locally

In the Terminal change current directory to the repository project.

Publish your artifact in the local repo. In follow command replace ALL-CAPS words with appropriate values:

```bash
mvn install:install-file -DgroupId=GROUP_ID -DartifactId=ARTIFACT_ID -Dversion=VERSION -Dfile=JAR_FILE_PATH
    -Dpackaging=jar -DgeneratePom=true -DlocalRepositoryPath=.  -DcreateChecksum=true
```

This would create local Maven repo with your file.

Commit new files to git repo and push them to GitHub.

#### Using repository from GitHub

In order to reference the artifact published this way Gradle project should include follow in the `build.gradle` file:
```groovy
repositories {
    mavenCentral()
    maven {
        url 'https://raw.githubusercontent.com/<GitHub_UserID>/<Priject_name>/<Branch_name>'
    }
}

dependencies {
    compile 'GROUP_ID:ARTIFACT_ID:VERSION'
}
```

Similarly include URL to repository and artifact identifier into POM file if Maven builds the project.
