# It-s-Probably-Nothing-Repo

<h3>pass-by-value vs pass-by-ref</h3>
* pass-by-value: pass in the variable that points to the address. Any changes to the variable/content of address will affect caller.
* pass-by-ref: pass in a new variable that points to the address. Changes to the variable will not reflect back to the caller. Only content change will reflect, because the two addresses are pointing to the same object.

http://www.hollischuang.com/
this person is indeed celery


# Short cut to check what jar doesn't exist in corp artifactory repo

<h3>The corp repo has limited third party jars. The developer built the project locally connecting to Jcenter & others. But in build pipeline we shouldn't. To check what jar are missing:</h3>
* 

```Gradle
publishing {
	publications {
		mavenJava(MavenPublication) {
			groupId group
			artifactId artifactName
			version = snapshotVersion
			artifact makeZip
			
			from components.java
			pom.withXml {
				Node pomNode = asNode()
				pomNode.remove(pomNode.get("dependencyManagement"))
			}
		}
	}
}
```
