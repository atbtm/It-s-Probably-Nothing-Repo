# It-s-Probably-Nothing-Repo 我自一口真气足

<h3>pass-by-value vs pass-by-ref</h3>
* pass-by-value: pass in the variable that points to the address. Any changes to the variable/content of address will affect caller.
* pass-by-ref: pass in a new variable that points to the address. Changes to the variable will not reflect back to the caller. Only content change will reflect, because the two addresses are pointing to the same object.

http://www.hollischuang.com/
this person is indeed celery


# Short cut to check what jar doesn't exist in corp artifactory repo

<h4>The corp repo has limited third party jars. The developer built the project locally connecting to Jcenter & others. But in build pipeline we shouldn't. To check what jar are missing:</h4>
<p>Remove the maven pom "dependencyManagement". Doing so will unroll/unmarshal dependencies.</p>
<a href="https://stackoverflow.com/a/37280943/6641032">Dependency Management VS dependencies</a>

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

Now running the Gradle publish again will start to complain all missing jars instead of just the missing root level hierachy pom.


# HTTP长连接和短连接
<a href="http://www.cnblogs.com/0201zcr/p/4694945.html">Link</a>
<p>长连结saves connection time -- only disconnect upon time out. But heavier weight; 短链接 load大量时常用</p>

# Front-end back-end decoupling
https://blog.csdn.net/piantoutongyang/article/details/65446892

# Why use Webpack
<a href="https://blog.andrewray.me/webpack-when-to-use-and-why/">Long/full story</a>
<p>It's a build tool to manage assets: Js, imgs, CSS etc.</p>
<h4>Ver. 1</h4>

```javascript
<script src="jquery.min.js"></script>  
<script src="jquery.some.plugin.js"></script>  
<script src="main.js"></script>  
```

<h4>Ver. 2</h4>

```javascript
// build-script.js
var scripts = [  
    'jquery.min.js',
    'jquery.some.plugin.js',
    'main.js'
].concat().uglify().writeTo('bundle.js');

// Everything our app needs!
<script src="bundle.js"></script>  
```

<a href="https://webpack.js.org/guides/getting-started/"><h4>Then the webpack way</h4></a>


# Queue, Dequeue, and why not stack
<p>Queue interface:</p>

| Queue         | Throw Excpetion| Return flag val|
| ------------- |:--------------:| --------------:|
| Insert        | add(e)         | offer(e)       |
| Remove        | remove()       | poll()         |
| Check         | elment()       | peek()         |

Notable implementations: ArrayList, Dequeue

<p>Deque class:</p>
<em>Two ended queue.</em>

| Deque         | Throw Excpetion             | Return flag val            |
| ------------- |:---------------------------:| --------------------------:|
| Insert        | addFirst(e)/addLast(e)      | offerFirst(e)/offerLast(e) |
| Remove        | removeFirst()/removeLast()  | pollFirst()/pollLast()     |
| Check         | getFirst()/getLast()        | peekFirst()/peekLast()     |

<p>Stack</p>
Stack inherits Vector class, which many consider an overuse of inheritance. It's now legacy stack impl. Now should use Deque when need a stack.
There are second opinions like <a href="http://baddotrobot.com/blog/2013/01/10/stack-vs-deque/">this</a>. Since Deque is NOT a stack. 

# Leaning Toothpick Syndrome
https://en.wikipedia.org/wiki/Leaning_toothpick_syndrome

# Reservior Sampling
When you are asked to pick N samples out of an unknown sized continous stream, how to ensure each sample is evenly distributed?

If the size (L) is given, then the probability of each element being picked is simply N/L. But when data size is unknown (like streaming), or very large (like certain DB), different technique has to be employed.

Reservoir Sampling: Fill the N places (base case). Then pick or discard a candidate such that the probability of getting picked is 1/i, where i is the index of candidate. Assume the previous N places are filled, the current candidate get selected this round should be N/i. If the candidate is picked, one of existing picked element is discarded randomly. Probability of a particular previously selected element will still be kept this around is then P(kept) = 1-P(discarded) = 1-P(new cand got picked)xP(the old element got discarded) = 1 - N/i x 1/N = 1 - 1/i. The probability of this old element surviving from start to the end of this round = P(the element survived previous rounds) x P(the element wasn't removed this round) = N/(i-1) x (1- 1/i) = N/(i-1) x (i-1)/i = N/i.

This scheme ensures that each and every candidate gets picked of equal chance, because: for any rounds, the prob of the new candidate gets picked is the same as previous elements staying.

<a href="https://gregable.com/2007/10/reservior-sampling.html">Greg Grothaus's explanation</a>
<a href="blog.jobbole.com/42550/">Chinese Vers.</a>

# Binary Incompatibility VS Source Incompatibility
- Binary Compatibility means that when something is updated, you continue to work without needing to even recompile. 
- Source Compatibility means that you need to recompile to keep things working, but you don't have to actually change the sources. 

<strong>Yes Binary + Yes Source:</strong>
Renaming private methods. Changing a method body in a way that continues to behave the same.

<strong>Yes Binary,  Not Source:</strong>
Adding new method overloads.  Since overload resolution is determined at compile time, adding new methods won't affect already-compiled binaries. But if you recompile, it's possible that you may bind to the new overloads.  (For example, see float.Equals)

<strong>Not Binary, Yes Source:</strong>
In this case, you just need to recompile your sources to keep working. The compiler will respond to the change in a corrective way. For example, consider removing a method overload. At a binary level, the method you're bound to is removed and so things fail. But if you recompile, the compiler may bind to another overload that's semantically equivalent, and so things keep working without you having to change any source.

<strong>Not Binary, Not Source:</strong>
A real breaking change. This requires clients to update their sources and recompile. For example, removing a method. 

https://blogs.msdn.microsoft.com/jmstall/2008/03/10/binary-vs-source-compatibility/

# Diff between SessionFactory get(..) and load(..)
- get runs the query right on the spot when the line is executed where load creates a proxy object, whose query only executes when the proxy's property is accessed. 

# Running Java on Docker? You are breaking the law
Very interesting article about legal implications when you use Oracle JRE in a docker image and why you should use OpenJDK for this purpose.
https://blog.takipi.com/running-java-on-docker-youre-breaking-the-law/

# Java Coding Style
<a href="https://google.github.io/styleguide/javaguide.html">Google Java Coding Guide</a>

# On Algorithm complexities

Asymptotically, log(n) grows no faster than log(n) (since it's the same), n, n^2, n^3 or 2^n.
So we can say f(n) is O(log(n)), O(n), O(n^2), O(n^3), and O(2^n). -- "Upper bounded by big O"

Asymptotically, log(n) grows at the same rate as log(n) (since it is the same).
So, we can say that f(n) is Θ( log(n) ) -- "Upper and lower bounded by Theta"

Lower bounded by Omega.

# Super Cool

Most liquids expand when they freeze, because usually solids are more tightly packed compared to that of the same mass of liquid. With few exeptions, including water. Water expands about 9% when freezes. If ice were denser than water.... Then no fish for you.

# Authentication
- Stateful server-side session + client-side cookie
VS
- Stateless token (usually just saved in local storage, also can be a cookie)
https://dzone.com/articles/cookies-vs-tokens-the-definitive-guide

# How to import Groovy methods from other file
```Groovy
Class executeMethods = ((GroovyClassLoader) this.class.classLoader).parseClass new File("\\pathToFile.groovy")
objectInstance = executeMethods.newInstance()
objectInstance.someMethodInThatFile()
```

Or maybe ?
```Groovy
evaluate(new File("../tools/Tools.groovy"))
```
https://stackoverflow.com/a/9154553/6641032

# Groovy MSSQL DB connection with Apache sql.sql
```Groovy
DBServer =  'localhost:59626'	// ### this pattern if you have multiple instance of DB server. The port number can be found from "Sql Server Configuration Manager" -> "SQL Server Network Configuration" then under the instance you want to connect to, TCP/IP, make sure it's enabled -> "IP Addresses" bottom "IPALL" 59626 is the TCP Dynamic Ports.
DBName =  'DBNAME'
DBUserid = 'username'
DBPassword = 'pw'

def DBConnectionString  = "jdbc:jtds:sqlserver://" + DBServer+ "/"+ DBName
def sqlDriver = "net.sourceforge.jtds.jdbc.Driver"	// some other driver
sqlConn = Sql.newInstance(DBConnectionString, DBUserid, DBPassword, sqlDriver)
```

# Initializing a class variable
Initialize -- 1) on spot when it's defined(can't handle exception). 2) in constructor. 3) initializer. Are equivalently handled by compiler.
https://stackoverflow.com/questions/1994218/should-i-instantiate-instance-variables-on-declaration-or-in-the-constructor

# How much calories it takes to digest food, and why 1 Cal in fat != 1 Cal in protein
https://www.precisionnutrition.com/digesting-whole-vs-processed-foods

# JVM memory model
https://betsol.com/2017/06/java-memory-management-for-java-virtual-machine-jvm/

# XML Marshalling/Unmarshalling Using JAXB
https://dzone.com/articles/jooby-framework-overview
@XmlRootElement specifies the root element for the XML document.

@XmlAttribute specifies the attribute for the root element.

@XmlElement specifies the sub-element for the root element.
Then call JAXBContext marshaller methods to process to/from doc.

# Java why are local variables uninitiated/have no default val?
https://stackoverflow.com/a/415714/6641032

# SSMS - how to connect to containerized sql server
(xxx.xx.xx.xxx,port). E.g. 127.0.0.1,1433. Must use sa + password to connect.
https://stackoverflow.com/questions/47984603/using-sql-server-management-studio-to-remote-connect-to-docker-container

# Java HashMap 扩容机制
https://blog.csdn.net/xiongxianze/article/details/70210437

# Self-fulfilling prophecy
https://en.wikipedia.org/wiki/Self-fulfilling_prophecy

# ANTD的圣诞节彩蛋
https://github.com/ant-design/ant-design/commit/00aebeb9756afecc884ad48486084836b9a2707a
https://github.com/ant-design/ant-design/issues/13848

# On RESTful API design
https://hackernoon.com/restful-api-designing-guidelines-the-best-practices-60e1d954e7c9
https://hackernoon.com/restful-api-design-step-by-step-guide-2f2c9f9fcdbf

# Data Access: raw JDBC vs JPA
https://medium.com/@Colin_But/spring-data-jpa-vs-raw-jdbc-e3492354d2ab

# Notes on Regex match group
```Java
userRepository.save(User.of("AAA", 10));
userRepository.save(User.of("BBB", 20));
userRepository.save(User.of("CCC", 30));
userRepository.save(User.of("DDD", 40));
userRepository.save(User.of("EEE", 50));
userRepository.save(User.of("FFF", 60));
userRepository.save(User.of("GGG", 70));
userRepository.save(User.of("HHH", 80));
userRepository.save(User.of("III", 90));
userRepository.save(User.of("JJJ", 100));
```
replace: new User\\("(.+)", (\d*)\\)

with:    User.of\\("\1", \2\\)

# A great discussion on trade-offs to use Java primitive type vs their wrapped objects
Time, space, and default value (depending on use cases, having a default value can be pro/con)

https://www.baeldung.com/java-primitives-vs-objects

# git log two dots vs three dots
https://stackoverflow.com/a/24186641/6641032

# A recursive tutorial on How To Google :D
http://www.lmgtfy.com/?q=how+to+google

# Great intro series to Git internals (that is actually comprehensible)
http://alblue.bandlem.com/Tag/gtotw/

# Why shouldn't you use global variables
https://softwareengineering.stackexchange.com/questions/148108/why-is-global-state-so-evil
