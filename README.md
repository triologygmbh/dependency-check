# dependency-check
An example on how to use the [OWASP Dependency-Check maven plugin](http://jeremylong.github.io/DependencyCheck/) 

## Contents
This example contains the following

- a maven multi-module project creating a WAR
- a module containing a dependency with CVE, that is *not* contained in the WAR (module [internal-module-with-vulnerability](internal-module-with-vulnerability/pom.xml))
- a module containing a dependency with CVE, that *is* contained in the WAR (module [published-module-with-vulnerability](published-module-with-vulnerability/pom.xml)) but is considered to be a false positive and is suppressed in [suppressed-cves.xml](suppressed-cves.xml)
- the final artefact (module [war](war/pom.xml)) that in turn contains dependencies with CVEs in `test`, `provided` and `compile` scopes.


## Maven and HTML Report
To execute the OWASP dependency check, just call

```sh
mvn clean install org.owasp:dependency-check-maven:check
```

A report containing a CVE is generated to `war/target/dependency-check-report.html`.

## Jenkins and XML Report
In order to integrate this in Jenkins, call

```sh
mvn clean install org.owasp:dependency-check-maven:check -Ddependency-check-format=XML
```

from your Jenkins job and use the [OWASP Dependecy-Check Jenkins plugin](https://wiki.jenkins-ci.org/display/JENKINS/OWASP+Dependency-Check+Plugin) to analyze `war/target/dependency-check-report.xml`
