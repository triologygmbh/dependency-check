# dependency-check
An example on how to use the [OWASP Dependency-Check maven plugin](http://jeremylong.github.io/DependencyCheck/).

This example belongs to an article published in [Java aktuell](http://www.ijug.eu/java-aktuell/das-magazin.html) 01/2017: 
[Automatic checks for vulnerabilities in Java project dependencies](https://www.triology.de/en/blog-entries/automatic-checks-for-vulnerabilities-in-java-project-dependencies).  
The original article (🇩🇪) can be found here: [Automatisierte Überprüfung von Sicherheitslücken in Abhängigkeiten von Java-Projekten](https://www.triology.de/wp-content/uploads/2017/01/01_2017-Java_aktuell-Johannes-Schnatterer_Automatisierte-Ueberpruefung-von-Sicherheitsluecken-in-Abhaengigkeiten-von-Java-Projekten.pdf).  

## Contents
This example contains the following

- a maven multi-module project creating a WAR
- a module containing a dependency with CVE, that is *not* contained in the WAR (module [internal-module-with-vulnerability](internal-module-with-vulnerability/pom.xml))
- a module containing a dependency with CVE, that *is* contained in the WAR (module [published-module-with-vulnerability](published-module-with-vulnerability/pom.xml)) but is considered to be a false positive and is suppressed in [suppressed-cves.xml](suppressed-cves.xml)
- the final artifact (module [war](war/pom.xml)) that in turn contains dependencies with CVEs in `test`, `provided` and `compile` scopes.


## Maven and HTML Report
To execute the OWASP dependency check, just call

```sh
mvn clean install org.owasp:dependency-check-maven:check
```

A report containing a CVE is generated to `war/target/dependency-check-report.html`.

## Jenkins and XML Report
The easiest way is to use this in Jenkins is the [pipeline plugin](https://wiki.jenkins-ci.org/display/JENKINS/Pipeline+Plugin) (formerly known as workflow plugin). Tested with version 2.4.
In Jenkins, do the following
- Install the [OWASP Dependecy-Check Jenkins plugin](https://wiki.jenkins-ci.org/display/JENKINS/OWASP+Dependency-Check+Plugin) (tested with version 1.4.3)
- Provide a maven tool called `M3.3'` and a JDK called `JDK8u102`, then
- Setup a new pipeline job in jenkins and add your repository URL,
- Optionally add a build parameter `RECIPIENTS` that contains a comma-separated list of all email recipients,
- Save and
- Build Now.
You should get an email notifying about the unstable build due to the Security warning.

If you prefer to rely on the Jenkins GUI, setup the following maven command

```sh
mvn clean install org.owasp:dependency-check-maven:check -Ddependency-check-format=XML
```

from your Jenkins job and use the [OWASP Dependecy-Check Jenkins plugin](https://wiki.jenkins-ci.org/display/JENKINS/OWASP+Dependency-Check+Plugin) to analyze `war/target/dependency-check-report.xml`
