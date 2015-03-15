# Resources #

  * [Sonatype OSS Maven Repository Usage Guide](https://docs.sonatype.org/display/Repository/Sonatype+OSS+Maven+Repository+Usage+Guide)
  * Sonatype OSS repositories: [staging](https://oss.sonatype.org/index.html#stagingRepositories), [search:com.googlecode.playn](https://oss.sonatype.org/index.html#nexus-search;gav~com.googlecode.playn~~~~)


# Preparation #

  1. Add your Sonatype credentials to your `~/.m2/settings.xml`:
```
<settings>
 <servers>
   <server>
     <id>sonatype-nexus-snapshots</id>
     <username>username</username>
     <password>password</password>
   </server>
   <server>
     <id>sonatype-nexus-staging</id>
     <username>username</username>
     <password>password</password>
   </server>
 </servers>
</settings>
```
  1. Install GPG
    * Debian/Linux:  `apt-get install gnupg`
    * OSX: Download from http://macgpg.sourceforge.net/
  1. Create a GPG key which will be used to sign the artifacts
```
gpg --gen-key
```


# Creating a new release #

  1. Deploy a SNAPSHOT release to [Sonatype OSS repository](https://oss.sonatype.org/index.html#nexus-search;gav~com.googlecode.playn~~~~) to confirm the SNAPSHOT release looks good:
```
cd ~/playn
git pull
mvn clean deploy
```
  1. For sanity, [clean up](http://maven.apache.org/plugins/maven-release-plugin/clean-mojo.html), undoing any release preparation:
```
mvn release:clean
```
  1. [Prepare](http://maven.apache.org/plugins/maven-release-plugin/examples/prepare-release.html) the release:
```
mvn release:prepare
```
  1. [Perform](http://maven.apache.org/plugins/maven-release-plugin/perform-mojo.html) the release:
```
mvn release:perform
```
  1. [Close](https://repository.sonatype.org/content/sites/maven-sites/nexus-maven-plugin/staging-close-mojo.html) the Nexus staging repository so it's available for use by Maven. (There's also a [manual alternative](https://oss.sonatype.org/index.html#stagingRepositories)).
```
mvn nexus:staging-close
```
  1. [Release](https://repository.sonatype.org/content/sites/maven-sites/nexus-maven-plugin/staging-release-mojo.html) (promote) a finished Nexus staging repository into a permanent Nexus repository for general consumption. (There's also a [manual alternative](https://oss.sonatype.org/index.html#stagingRepositories)).
```
mvn nexus:staging-promote
```